# Setting Up A Local Instance #

Unfortunately, I do not know your local tech stack, so I cannot make detailed recommendations. However, I documented the pages/links I used to get my Macbook Pro + Mavericks ready to run a near-exact clone of the live server/code.


## Getting a local PHP dev environment set up:
A very straighforward Nginx and PHP-FPM server, with MySQL. I use `homebrew` as my package manager, so:

https://rtcamp.com/tutorials/mac/osx-brew-php-mysql-nginx/

_useful command_:
`sudo lsof -i TCP:9000` <— see what’s running on port 9000 (where PHP-FPM runs)

http://www.handcraftedsoftware.net/articles/installing-nginx-php-fpm-and-apc-on-mac-os-x

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
