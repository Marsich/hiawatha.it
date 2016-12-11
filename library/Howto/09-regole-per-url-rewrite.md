# Regole per URL rewrite

Questa pagina contiene le regole di "URL rewrite" per alcuni Content Management System, wiki, webmail e altri applicativi web.

****

### Banshee


Usa la seguente configurazione per i tuoi siti in [Banshee](http://www.banshee-php.org/):

```
UrlToolkit {
    ToolkitID = banshee
    RequestURI isfile Return
    Match ^/(css|files|fonts|images|js)(/|$) Return
    Match ^/(favicon.ico|robots.txt)$ Return
    Match [^?]*(\?.*)? Rewrite /index.php$1
}
```

****

### CakePHP

Usa la seguente configurazione per i tuoi siti in [CakePHP](http://cakephp.org/):

```
UrlToolkit {
    ToolkitID = cakephp
    RequestURI exists Return
    Match .* Rewrite /index.php
}
```

****

### CodeIgniter

Usa la seguente configurazione per i tuoi siti in [CodeIgniter](https://codeigniter.com/):

```
UrlToolkit {
    ToolkitID = codeigniter
    RequestURI exists Return
    Match ^/(index\.php|images|robots\.txt) Return
    Match .* Rewrite /index.php
}
```

****

### Concrete5

Usa la seguente configurazione per i tuoi siti in [Concrete5](http://www.concrete5.org/):

```
UrlToolkit {
    ToolkitID = concrete5
    RequestURI exists Return
    Match ^/index.php Return
    Match ^/(.*)\?(.*) Rewrite /index.php/$1&$2
    Match ^/(.*) Rewrite /index.php/$1
}
```

****

### Contao

Usa la seguente configurazione per i tuoi siti in [Contao](https://contao.org/):

```
UrlToolkit {
    ToolkitID = contao
    RequestURI exists Return
    Match /contao/(.*)\?(.*) Rewrite /contao/index.php?id=$1&$2
    Match /contao/(.*) Rewrite /contao/index.php?id=$1
}
```

****

### CraftCMS

Use the following configuration for your [CraftCMS](https://buildwithcraft.com/) websites:

```
UrlToolkit {
    ToolkitID = craftcms
    RequestURI exists Return
    Match ^/(.*)\?(.*) Rewrite /index.php?p=$1&$2
    Match ^/(.*) Rewrite /index.php?p=$1
}
```

****

### DokuWiki

Usa la seguente configurazione per i tuoi siti in [DokuWiki](http://www.dokuwiki.org/):

```
UrlToolkit {
    ToolkitID = dokuwiki
    Match ^/(bin|conf|data|inc)/ DenyAccess
    Match ^/_media/(.*)\?(.*) Rewrite /lib/exe/fetch.php?media=$1&2
    Match ^/_media/(.*) Rewrite /lib/exe/fetch.php?media=$1
    Match ^/_detail/(.*)\?(.*) Rewrite /lib/exe/detail.php?media=$1&$2
    Match ^/_detail/(.*) Rewrite /lib/exe/detail.php?media=$1
    Match ^/_export/([^/]+)/(.*) Rewrite /doku.php?do=export_$1&id=$2
    Match ^/$ Rewrite /doku.php
    RequestURI exists Return
    Match /(.*)\?(.*) Rewrite /doku.php?id=$1&$2
    Match /(.*) Rewrite /doku.php?id=$1
}
```

****

### Drupal

Usa la seguente configurazione per i tuoi siti in [Drupal](https://drupal.org/):

```
UrlToolkit {
    ToolkitID = drupal
    RequestURI isfile Return
    Match ^/favicon.ico$ Return
    Match /(.*)\?(.*) Rewrite /index.php?q=$1&$2
    Match /(.*) Rewrite /index.php?q=$1
}
```

****

### GetSimple

Usa la seguente configurazione per i tuoi siti in [GetSimple](http://get-simple.info/):

```
UrlToolkit {
    ToolkitID = getsimple
    Match ^/(data/uploads|data/thumbs)/ Skip 1
    Match ^/(data|plugins|backups)/ DenyAccess
    RequestURI exists Return
    Match ^(.*)*/([A-Za-z0-9\-]+)/?$ Rewrite /index.php?id=$2
}
```
****

### Git HTTP backend

Usa la seguente configurazione per i tuoi siti in [Git HTTP backend](http://git-scm.com/docs/git-http-backend):

```
CGIextension = cgi

VirtualHost {
    ...
    ExecuteCGI = yes
    NoExtensionAs = cgi
    EnablePathInfo = yes
    ScriptAlias = /git:/usr/libexec/git-core/git-http-backend
    Setenv GIT_PROJECT_ROOT = /var/www/git
    Setenv GIT_HTTP_EXPORT_ALL =
}
```

****

### H5ai

Use the following configuration for your [h5ai](https://larsjung.de/h5ai/) websites:

```

UrlToolkit {
    ToolkitID = h5ai
    RequestURI isfile Return
    Match .* Rewrite /_h5ai/public/index.php
}
```

****

### Habari

Usa la seguente configurazione per i tuoi siti in [Habari](http://habariproject.org/):

```
UrlToolkit {
    ToolkitID = habari
    Match ^/(system/(classes|locale|schema|$)) Rewrite /index.php
    RequestURI exists Return
    Match [^?]*(\?.*)? Rewrite /index.php$1
}
```

****

### jCore

Usa la seguente configurazione per i tuoi siti in [jCore](http://jcore.net/):

```
UrlToolkit {
    ToolkitID = jcore
    RequestURI exists Return
    Match /(.*)\?(.*) Rewrite /index.php?path=$1&$2
    Match /(.*) Rewrite /index.php?path=$1
}
```

****

### Joomla!

Usa la seguente configurazione per i tuoi siti in [Joomla!](http://www.joomla.org/):

```
UrlToolkit {
    ToolkitID = joomla
    Match base64_encode[^(]*\([^)]*\) DenyAccess
    Match (<|%3C)([^s]*s)+cript.*(>|%3E) DenyAccess
    Match GLOBALS(=|\[|\%[0-9A-Z]{0,2}) DenyAccess
    Match _REQUEST(=|\[|\%[0-9A-Z]{0,2}) DenyAccess
    Match ^/index\.php Return
    RequestURI exists Return
    Match .* Rewrite /index.php
}
```

Also set 'HTTPAuthToCGI = yes' in the virtual host configuration.

****

### Kohana

Usa la seguente configurazione per i tuoi siti in [Kohana](http://kohanaframework.org/):

```
UrlToolkit {
    ToolkitID = kohana
    Match ^/(application|modules|system) DenyAccess
    RequestURI exists Return
    Match ^/(.*) Rewrite /index.php?kohana_uri=$1
}
```

Set $config['site_domain'] to '/' and $config['index_page'] to ''.


****

### Laravel

Usa la seguente configurazione per i tuoi siti in [Laravel](http://laravel.com/):

```
UrlToolkit {
    ToolkitID = laravel
    RequestURI exists Return
    Match [^?]*(\?.*)? Rewrite /index.php$1
}
```

****

### MediaWiki

Usa la seguente configurazione per i tuoi siti in [MediaWiki](http://www.mediawiki.org/wiki/MediaWiki):

```
UrlToolkit {
     ToolkitID = mediawiki
     RequestURI exists Return
     Match /wiki/(.*) Rewrite /index.php?title=$1
 }

 VirtualHost {
     ...
     TriggerOnCGIstatus = false
     UseToolkit = mediawiki
 }
```

Set $wgArticlePath to "/wiki/$1" in LocalSettings.php. If you often see white pages,
set $wgEnableParserCache and $wgCachePages to false. Yes, this is due to a bug in MediaWiki's caching routines.

****

### MODx

Usa la seguente configurazione per i tuoi siti in [MODx](http://modx.com/):

```
UrlToolkit {
    ToolkitID = modx
    RequestURI exists Return
    Match ^/(.*)$ Rewrite /index.php?q=$1
}
```

****

### Nibbleblog

Usa la seguente configurazione per i tuoi siti in [Nibbleblog](http://www.nibbleblog.com/):

```
UrlToolkit {
    ToolkitID = nibbleblog
    Match ^/admin$ Rewrite /admin.php?controller=user&action=login
    Match ^/category/([^/]+)/page-([0-9]+)$ Rewrite /index.php?controller=blog&action=view&category=$1&number=$2
    Match ^/category/([^/]+)/$ Rewrite /index.php?controller=blog&action=view&category=$1&number=0
    Match ^/tag/([^/]+)/page-([0-9]+)$ Rewrite /index.php?controller=blog&action=view&tag=$1&number=$2
    Match ^/tag/([^/]+)/$ Rewrite /index.php?controller=blog&action=view&tag=$1&number=0
    Match ^/page-([0-9]+)$ Rewrite /index.php?controller=blog&action=view&number=$1
    Match ^/post/([^/]+)/$ Rewrite /index.php?controller=post&action=view&post=$1
    Match ^/post-([0-9]+)/(.*)$ Rewrite /index.php?controller=post&action=view&id_post=$1
    Match ^/page/([^/]+)/$ Rewrite /index.php?controller=page&action=view&page=$1
    Match ^/feed/$ Rewrite /feed.php
    Match ^/([^/]+)/$ Rewrite /index.php?controller=page&action=$1
}
```

****

### October

Use the following configuration for your [October](https://octobercms.com/) websites:

```
UrlToolkit {
    ToolkitID = october
    Match /themes/.*/(layouts|pages|partials)/.*.htm Rewrite /index.php
    Match /uploads/protected/.* Rewrite /index.php
    RequestURI file Return
    Match .* Rewrite /index.php
}
```

****

### OpenCart

Use the following configuration for your [OpenCart](http://www.opencart.com/) websites:

```
UrlToolkit {
  ToolkitID = opencart
  Match ^/sitemap.xml$ Rewrite /index.php?route=feed/google_sitemap
  Match ^/googlebase.xml$ Rewrite /index.php?route=feed/google_base
  Match ^/system/download/.* Rewrite /index.php?route=error/not_found
  RequestURI exists Return
  Match \.(ico|gif|jpg|jpeg|png|js|css) Return
  Match ([^?]*)(\?(.*))? Rewrite /index.php?_route_=$1&$3
}
```

****

### Phalcon

Use the following configuration for your [Phalcon](http://www.phalconphp.com/) websites:

```
UrlToolkit {
    ToolkitID = phalcon
    Match ^/public/ Skip 1
    Match ^/(.*) Rewrite /public/$1 Continue
    RequestURI exists Return
    Match (.*)\?(.*) Rewrite $1&$2 Continue
    Match ^/public/(.*) Rewrite /public/index.php?_url=/$1
}
```

****

### phpBB

Use the following configuration for your [phpBB](https://www.phpbb.com/) websites:

```
UrlToolkit {
  ToolkitID = phpBB
  RequestURI exists Return
  Match [^?]*(\?.*)? Rewrite /app.php$1
}
```

****

### phpSQLiteCMS

Usa la seguente configurazione per i tuoi siti in [phpSQLiteCMS](http://phpsqlitecms.net/):

```
UrlToolkit {
    ToolkitID = phpsqlitecms
    RequestURI exists Return
    Match ^/(.*) Rewrite /index.php?qs=$1
}
```

****

### Pico

Use the following configuration for your [Pico](http://picocms.org/) websites:

```
UrlToolkit {
    ToolkitID = picocms
    RequestURI isfile Return
    Match .* Rewrite /index.php
}
```

****

### PluXml

Use the following configuration for your [PluXml](http://www.pluxml.org/) websites:

```
UrlToolkit {
    ToolkitID = pluxml
    Match ^/data/configuration DenyAccess
    RequestURI exists Return
    Match ^/tag/(.*)$ Rewrite /index.php?tag/$1
    Match ^/categorie([A-Za-z0-9\-]+)/(.*)$ Rewrite /index.php?categorie$1/$2
    Match ^/article([A-Za-z0-9\-]+)/(.*)$ Rewrite /index.php?article$1/$2
    Match ^/feed/rss/(.*)$ Rewrite /feed.php?rss$1
    Match ^/feed/rss$ Rewrite /feed.php?rss$1
    Match ^/archives/(.*)/(.*)$ Rewrite /index.php?archives/$1/$2
    Match ^/static([A-Za-z0-9\-]+)/(.*)$ Rewrite /index.php?static$1/$2
}
```

****

### ProcessWire

Use the following configuration for your [ProcessWire](http://processwire.com/) websites:

```
UrlToolkit {
    ToolkitID = processwire
    Match /site/assets/(cache|logs|backups|sessions|config|install|tmp)($|/.*$) DenyAccess
    Match /site/assets.*/-.+/.* DenyAccess
    Match /(wire|site)/(config|index\.config|config-dev)\.php$ DenyAccess
    Match /(wire|site)/templates-admin($|/|/.*\.(php|html?|tpl|inc))$ DenyAccess
    Match /site/templates($|/|/.*\.(php|html?|tpl|inc))$ DenyAccess
    Match /site/assets($|/|/.*\.php)$ DenyAccess
    Match /wire/(core|modules)/.*\.(php|inc|tpl|module|info\.json)$ DenyAccess
    Match /site/modules/.*\.(php|inc|tpl|module|info\.json)$ DenyAccess
    RequestURI exists Return
    Match ^/(.*\/?)?\?(.*)$ Rewrite /index.php?it=$1&$2
    Match ^/(.*)$ Rewrite /index.php?it=$1
}
```

****

### Pydio

Usa la seguente configurazione per i tuoi siti in [Pydio](http://pyd.io/):

```
UrlToolkit {
    ToolkitID = pydio
    Match ^/data DenyAccess
    RequestURI exists Return
    Match ^/shares Rewrite /dav.php
    Match ^/api Rewrite /rest.php
    Match ^/user Rewrite /index.php?get_action=user_access_point
}
```

During the installation process, you need to set MaxUrlLength = 3000.


****

### RoundCube

Usa la seguente configurazione per i tuoi siti in [RoundCube](http://roundcube.net/):

```
UrlToolkit {
    ToolkitID = roundcube
    Match (/.*\.inc|^_.*) DenyAccess
    Match /mail/logs/.* DenyAccess
    Match /mail/temp/.* DenyAccess
    Match /mail/config/.* DenyAccess
}
```

****

### SilverStripe

Usa la seguente configurazione per i tuoi siti in [SilverStripe](http://www.silverstripe.com/):

```
UrlToolkit {
    ToolkitID = silverstripe
    RequestURI isfile Return
    Match (.*)\?(.*) Rewrite $1&$2 Continue
    Match ^/(.*) Rewrite /sapphire/main.php?url=$1
}

VirtualHost {
    ...
    TriggerOnCGIstatus = no
    UseToolkit = silverstripe
}
```

****

### Symfony2

Usa la seguente configurazione per i tuoi siti in [Symfony2](http://symfony.com/):

```
UrlToolkit{
    ToolkitID = Symfony2
    RequestURI isfile Return
    Match .* Rewrite /app.php
}
```

****

### Symphony

Usa la seguente configurazione per i tuoi siti in [Symphony](http://www.getsymphony.com/):

```
UrlToolkit {
    ToolkitID = symphony
    Match ^/manifest/.* DenyAccess
    Match ^/workspace/utilities/.*\.xsl$ DenyAccess
    Match ^/workspace/pages/.*\.xsl$ DenyAccess
    Match ^/.*\.sql$ DenyAccess
    Match ^/favicon.ico$ Return
    Match ^/image\/(.+\.(jpg|gif|jpeg|png|bmp))$ Rewrite /extensions/jit_image_manipulation/lib/image.php?param=$1
    Match ^/symphony\/?$ Rewrite /index.php?mode=administration
    RequestURI exists Return
    Match ^/(.*)\?(.*) Rewrite /$1&$2 Continue
    Match ^/symphony(\/(.*\/?))?$ Rewrite /index.php?symphony-page=$1&mode=administration
    Match ^/(.*\/?)$ Rewrite /index.php?symphony-page=$1
}
```

****

### Textpattern CMS

Usa la seguente configurazione per i tuoi siti in [Textpattern CMS](http://textpattern.com/):

```
UrlToolkit {
    ToolkitID = textpattern
    RequestURI exists Return
    Match ^/(files|images|js|res)(/|$) Return
    Match ^/(favicon.ico|robots.txt|sitemap.xml)$ Return
    Match [^?]*(\?.*)? Rewrite /index.php$1
}
```

****

### TYPO3

Usa la seguente configurazione per i tuoi siti in [TYPO3](http://typo3.org/):

```
UrlToolkit {
    ToolkitID = typo3
    RequestURI exists Return
    Match .* Rewrite /index.php
}
```

****

### WolfCMS

Usa la seguente configurazione per i tuoi siti in [WolfCMS](http://www.wolfcms.org/):

```
UrlToolkit {
    ToolkitID = wolfcms
    Match ^/site/install/index.html$ Rewrite /site/install/index.php?rewrite=1
    Match ^/site/install/index.php$ Rewrite /site/install/index.php?rewrite=1
    Match ^/site/install/$ Rewrite /site/install/index.php?rewrite=1
    RequestURI exists Return
    Match ^/site/admin(.*)$ Rewrite /site/admin/index.php?$1
    Match ^/site(.*)$ Rewrite /site/index.php?WOLFPAGE=$1
}
```

The 3 match rules above RequestURI are for clean URL installation. They should be removed after installation.


****

### WordPress

Usa la seguente configurazione per i tuoi siti in [WordPress](http://wordpress.org/):

```
UrlToolkit {
    ToolkitID = wordpress
    RequestURI exists Return
    Match [^?]*(\?.*)? Rewrite /index.php$1
}
```

```
UrlToolkit {
    ToolkitID = wp-multi-subdir
    Match ^/index\.php$ Return
    Match ^/([_0-9a-zA-Z-]+/)?wp-admin$ Redirect /$1wp-admin/
    RequestURI exists Return
    Match ^/([_0-9a-zA-Z-]+/)?(wp-(content|admin|includes).*) Rewrite /$2
    Match ^/([_0-9a-zA-Z-]+/)?(.*\.php)$ Rewrite /$2
    Match ^/[_0-9a-zA-Z-]+(/wp-.*) Rewrite /$1    # if not present 404 - error is displayed
    Match .* Rewrite /index.php
}
```

For a complete instruction about how to use Wordpress with Hiawatha, read [Hosting WordPress with Hiawatha](http://dotbalm.org/hosting-wordpress-with-hiawatha/) by Chris Wadge.

****

### Xenforo

Use the following configuration for your [Xenforo](https://xenforo.com/) websites:

```
UrlToolkit {
    ToolkitID = xenforo
    RequestURI exists Return
    Match [^?]*(\?.*)? Rewrite /index.php$1
}
```

****

### Yii

Usa la seguente configurazione per i tuoi siti in [Yii](http://www.yiiframework.com/):

```
UrlToolkit {
    ToolkitID = yii
    RequestURI exists Return
    Match .* Rewrite /index.php
}
```

****

### Zend

Usa la seguente configurazione per i tuoi siti in [Zend](http://www.zend.com/):

```
UrlToolkit {
    ToolkitID = zend
    Match ^/.*\.(js|ico|gif|jpg|jpeg|png|css|svg)(/|$) Return
    Match .* Rewrite /index.php
}

VirtualHost {
    ...
    WebsiteRoot = /path/to/website/public
    UseToolkit = zend

    # Required so that Zend can find the application root directory
    Setenv APPLICATION_PATH = /path/to/website
    # Optional, for more verbose error messsages
    Setenv APPLICATION_ENV = development
}
```


Pagina originale: [https://www.hiawatha-webserver.org/howto/url_rewrite_rules](https://www.hiawatha-webserver.org/howto/url_rewrite_rules)