# Assets #

In 07/2013 we migrated all static assets (users' portfolio images,
site's javascript, css, and image files, etc) to Amazon's S3 and
Cloudfront CDN service. 

Credentials for the AWS console are also in the
[Dev/Ops](https://docs.google.com/a/theidealists.com/spreadsheet/ccc?key=0AjVZk7pDeorYdGhVa0Z1R3pJX2lhM1VnZWlqYzhuN1E#gid=0)
spreadsheet. 

### Deployments and S3 configuration

As mentioned in the Code and Deployments section, all site-related
assets are processed (if they're LESS or JS files) and then uploaded to
an S3 bucket upon deployment.

### CDN and Caching Issues

Cloudfront can present some issues with files of the same name, as it
has a strong cache (by design). Often, it's too laborious to put
multiple manual "invalidations" through the web interface; hence, when
a critical change occurs (significant code changes, new release) we bump
the "version" number, forcing a new S3 bucket to be created and a fresh
Cloudfront cache.

The version number is located in the `bootstrap.php` file,
`AWS.assets_version` section, as below:

```php
Configure::write(array(
    'AWS.key'=>'xxxxxxxxxxxxxxxxxx',
    'AWS.secret'=>'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
    'base_assets_url' => 'cache_assets/',
    'AWS.cdn_url' =>
    'https://d1p84xostbzulx.cloudfront.net',
    //dev    'AWS.cdn_url' =>
    'https://d3ske6itjq7vhb.cloudfront.net',
    'AWS.assets_version' => 'v3.5.1'.$hash,
));
```
