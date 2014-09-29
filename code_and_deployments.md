# Code and Deployments

## Code and Deployment Topics ##
* Version Control
* Migrations/Migration Management
* Bitbucket/Remote Repo
* Branching Scheme
* Blog
* Deployments & Config
    * SSH Keys
    * Fabric / Manual Deployment
    * Bootstrap Files on Subdomains
    * AssetCompress & S3

## Version Control
We use _Git_ for version control, and most of our development and production code is on _Bitbucket_. QA code is based in a private _Github_ repo (because SauceLabs has a free layer linked with _GitHub_). Access passwords are located in the [Dev/Ops](https://docs.google.com/a/theidealists.com/spreadsheet/ccc?key=0AjVZk7pDeorYdGhVa0Z1R3pJX2lhM1VnZWlqYzhuN1E#gid=0) document. Both _Bitbucket_ and _GitHub_ have instructions on setting up your SSH keys.

## Migrations/Migration Management
Schema migrations and data updates are controlled through the [_Migrations_ plugin](https://github.com/CakeDC/migrations). The long list of migrations can be found in `app/Config/Migration/` and the base schema in `app/Config/Schema/schema.php`. 

## Bitbucket/Remote Repo
The main repo for The IdeaLists is located at [https://bitbucket.org/idldev/theidealists.com](https://bitbucket.org/idldev/theidealists.com).

## Branching Scheme
While this was outlined in the **Technical Overview** of the Due Diligence documents (shared Dropbox folder), some further details are included here:

### Branching Scheme: Past Work
In the past, when we had multiple developers, we adopted the following scheme:
* When a developer was assigned a new task, they created a _feature branch_ with the JIRA id, e.g. `feature/NEW-1211`. 
* When finished, and ready to be tested, the developer would merge into a subdomain branch (e.g. `hoth` for `hoth.theidealists.com`) for initial QA.
* When preparing for a deployment/release, the developer would then merge their _feature_ or whole subdomain branch into a core branch with other developers' code (e.g. `develop` for `dev.theidealists.com`, or- if a large release, `release/{version}.{subversion}` which maps to `staging.theidealists.com`). 
* Full QA, acceptance testing, and the like would occur, final fixes committed, and then merge into `master` for production deployment.

### Branching Scheme: Current Work
Now, with only one developer at work but several different large-scale features to work on, it's pretty much as follows:

* **tatooine**: all PAMM related code
* **hoth**: all Matchmaking related code
* **endor**: SOW data edits in-progress
* **release/3.5**: the current release branch, where hotfixes are staged
* **master**: production code.

## Blog
The blog is [an independent repo](https://bitbucket.org/idldev/idl-blog), as the main code is _WordPress_ and updates are far less frequent.

## Deployments & Config

### SSH Keys
As noted in the **Server** section of this document, the servers accept SSH on a non-standard port. For ease of use, we've set up some shared SSH keys and an SSH config file on the servers to facilitate communication. Copies of the SSH keys are in the top level of the _git_ repo; the SSH config is in the shared Google Drive folder (link).

### Fabric / Manual Deployment
The basic mechanics of deployments are handled by the `fabfile.py`. The general list of actions is:
* Make sure _git_ etc. is installed on the target server
* make sure _ssh keys_ are in the proper place
* clone the specified repo from the remote
* perform necessary update actions (compress assets, upload to S3, generate sitemap, run migrations, run ACL updates, etc.)
* place crontabs
* update Solr, restart Solr
* clean up old versions (we keep the last few in case of manual rollbacks)
* restart `nginx` and `php-fpm`

A developer can run a deployment directly from their local:
```bash
fab -i /path/to/key [development|staging|test|production] [init|deploy[:update_schema=True,update_acl=True]] 
```

We have **Jenkins** monitoring our _Bitbucket_ repo and should it detect changes to certain branches (`master`, `develop`, `release/*`, `hoth`, `tatooine`, `endor`, `dagobah`, etc.) it will automate the release.

Therefore, typical workflow is to push to a specfic repo and the deployment will follow automatically.

### Bootstrap Files on Subdomains
In order to make testing easier on the various branches/subdomains, the main `app/Config/bootstrap.php` will attempt to include a _subdomain-or-branch specific_ bootstrap file (e.g. `bootstrap-develop.php`) which includes custom config and overrides. 

The system knows which bootstrap-{server} to look for from an ENV variable set up on each domain/subdomains `nginx` config... 

### AssetCompress & S3
As mentioned above, one step in deployment is compressing assets and uploading to S3. We use the [AssetCompress](https://github.com/markstory/asset_compress) plugin and some `Node.js` helpers to process the LESS files and combine JS and CSS files into minified, combined versions. 

These, as well as the images, are then uploaded to S3 where they can be served via the CloudFront CDN. 

### Deploying the Blog
As mentioned, the blog is a separate repo. Deployments are not monitored by **Jenkins** nor automated with **Fabric**. In a very simple workflow, edits are made locally, and _SSH_ or _SCP_ is used to update the code/files on the server. 

It is the working developer's responsibility to also push edits to the remote repo. 

Automating this was on the long-term but low-priority TO-DO list. 

