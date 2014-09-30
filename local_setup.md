# Setting Up A Local Instance #

Unfortunately, I do not know your local tech stack, so I cannot make detailed recommendations. However, I documented the pages/links I used to get my Macbook Pro + Mavericks ready to run a near-exact clone of the live server/code.

### Trouble running AssetCompress locally on Mac
This might be it:

```bash
#in /app/Plugin/AssetCompress/Lib/Filter/LessCss.php, I found this:

protected $_settings = array(
      'ext' => '.less',
      'node' => 'node',
      'node_path' => '/usr/lib/node_modules'
 );
```

rather than edit the source code, I went to add a symbolic link:

`sudo ln -s /usr/local/lib/node_modules /usr/lib/node_modules`
