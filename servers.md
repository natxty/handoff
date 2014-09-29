# Servers #

## General Server Notes ##
Access to the servers is available via the [Rackspace MyCloud Web Interface](https://mycloud.rackspace.com/) or via SSH tools. 

SSH ports and passwords located in the shared Dev/Ops Google Doc.

All domains are purchased through GoDaddy. GoDaddy admin credentials are also in the Google Doc.

## Main Servers
We run our main production + development server off Rackspace.
There are two servers:
* `idlrack`: all production code (main site + blog)
* `idlstage`: most of the development subdomains

There is a small AWS `t1.micro` server, `idlmicro`, that hosts some python tools and a few older domain snapshots.

### Rackspace Notes

* Prod server (`idlrack`) has daily full snapshots
* Several status monitors configured here (CPU, http, etc.)
* Both `idlrack` and `idlstage` run Ubuntu 12.04 - which is reaching its _End-of-Life_ date. Upgrading to 14.04 should happen soon.

### idlrack
Runs the following domains:
* [theidealists.com](http://theidealists.com): production site
* [blog.theidealists.com](http://blog.theidealists.com): wordpress blog.
* Java, Tomcat, Solr installed here. Tomcat found at, e.g [dev.theidealists.com:9090/manager/html](http://theidealists.com:9090/manager/html)


### idlstage
* **blog-dev.theidealists.com** (blog development)
* **dagobah.theidealists.com** (dev site, tied to `dagobah` branch)
* **demo.theidealists.com**: in-progress "empty" demo instance for sales. stopped usage.
* **dev.theidealists.com** (dev site, tied to `develop` branch)
* **endor.theidealists.com** (dev site, tied to `endor` branch)
* **hoth.theidealists.com** (dev site, tied to `hoth` branch, where most the `matchmaking` code lives)
* **marketing.theidealists.com**: an early attempt to make Landing Pages and SEM stuff separate from the main branches of development. Silex/PHP app. Not used.
* **ops.theidealists.com**: _phpMyAdmin_ instance, tied to a clone of the production database, refreshed daily. So the Operations crew could run simple SQL searches on the data.
* **saleucami.theidealists.com** (dev site, tied to `saleucami` branch)
* **staging.theidealists.com** (final QA site, where `release/*` branches are deployed)

#### Tech Stack Notes
* Java, Tomcat, Solr installed here. Tomcat found at, e.g [dev.theidealists.com:9090/manager/html](http://dev.theidealists.com:9090/manager/html)
* Jenkins/Thucydides runs off `idlstage`
* Python libs here, most specifically: NLTK, NumPy, etc.

### idlmicro
* [beta.theidealists.com](http://beta.theidealists.com/): First major redesign, 07.2013-era. Test accounts only.
* [old.theidealists.com](http://old.theidealists.com/): pre-2013 website. DB is a 2013 production snapshot. _Double check email is disabled before playing_.
* [test.theidealists.com/](http://test.theidealists.com/): early 2014 snapshot.



