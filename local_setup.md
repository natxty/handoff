# Setting Up A Local Instance #

Unfortunately, I do not know your local tech stack, so I cannot make detailed recommendations. However, I documented the pages/links I used to get my Macbook Pro + Mavericks ready to run a near-exact clone of the live server/code.

## Getting a local PHP dev environment set up:
A very straighforward Nginx and PHP-FPM server, with MySQL. I use `homebrew` as my package manager, so:

https://rtcamp.com/tutorials/mac/osx-brew-php-mysql-nginx/

_useful command_:
`sudo lsof -i TCP:9000` <— see what’s running on port 9000 (where PHP-FPM runs)

http://www.handcraftedsoftware.net/articles/installing-nginx-php-fpm-and-apc-on-mac-os-x

### Setting up Solr
The basics on Mac:
http://risnandar.wordpress.com/2013/09/08/how-to-install-and-setup-apache-lucene-solr-in-osx/

Pretty good review on building the data import for mysql:
http://entropytc.com/importing-a-mysql-database-into-apache-solr/

But you'll need to get to running Solr via tomcat on port 9090:
http://jarrettmeyer.com/blog/2012/12/05/setting-up-solr-on-tomcat-in-mac-os-x

#### configure SOLR
Using homebrew, here's where the conf files are:
`/usr/local/Cellar/solr/{X.x.x}/libexec/example/solr/collection1/conf`
.. where X.x.x. is your Solr version

some things:
Solr start path/folders
`/usr/local/Cellar/solr/{X.x.x}/libexec/example`

from http://craiccomputing.blogspot.com/2012/07/installing-apache-tomcat-on-mac-os-x.html
To start/stop the server from the command line you use the catalina shell script
```
$ /usr/local/Cellar/tomcat/7.0.52/bin/catalina run
[...]
$ /usr/local/Cellar/tomcat/7.0.52/bin/catalina stop
```

****

## Getting the active code
If your system is set up to serve, the easiest way to get up & running is:

```bash
cd path/to/local/instance
git clone git@bitbucket.org:idldev/theidealists.com.git
```

you'll need a database, and I'd recommend copying one of the remote, development databases:

```bash
ssh ${SSH_HOST} "mysqldump -u${DBUSER} -p${DBPASS} ${DBNAME}" > temp.sql
mysql -uroot ${TARGET_DB} < temp.sql
```

or something similar... saving you from having to run multiple migrations.

****
Otherwise, you can just refresh your local git, run migrations, update the ACL, as below:

```bash
# update your code
cd /path/to/local/instance
git pull origin {branch}

# run migrations, acl update, asset compress….
export APPLICATION_ENV='local' && app/Console/cake Migrations.migration run all
export APPLICATION_ENV='local' && app/Console/cake AclExtras.AclExtras aco_update 
export APPLICATION_ENV='local' && app/Console/cake AssetCompress.AssetCompress build
```


## Errors and Issues

### Trouble running AssetCompress locally on Mac
When running the Asset.Build for The Idealists locally, I hit a significant error. Eventually I found the solution, and this might help you:

```bash
#in /app/Plugin/AssetCompress/Lib/Filter/LessCss.php, I found this:

protected $_settings = array(
      'ext' => '.less',
      'node' => 'node',
      'node_path' => '/usr/lib/node_modules'
 );
```

With `node_path` being incorrect for the local Mac instance. But, rather than edit the source code, I just added a symbolic link:

`sudo ln -s /usr/local/lib/node_modules /usr/lib/node_modules`

### Fixing the mysql "no default value" error, on local
Running the site locally, you might receive `No Default Value` database errors on certain attempts to INSERT or UPDATE (e.g. when you try to save an Opportunity off the `/creative-talent` form). 

This has to do with certain default settings on Mac. While there might be a better/more global solution, I simply added: `SET GLOBAL sql_mode = '';` to my database update script as a final step, which allows a permissive `SQL.mode` which better replicates the production database behavior.


