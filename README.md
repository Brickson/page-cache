[![Latest Stable Version](http://img.shields.io/packagist/v/mmamedov/page-cache.svg)](https://packagist.org/packages/mmamedov/page-cache) [![License](https://img.shields.io/packagist/l/mmamedov/page-cache.svg)](https://packagist.org/packages/mmamedov/page-cache) 

Full-page PHP Caching library
----
PageCache is a lightweight PHP library for full page cache, works out of the box with zero configuration. Use it when you need a simple yet powerfull file based PHP caching solution. Page caching for mobile devices is built-in.

No Database calls
----
Once page is cached, there are no more database calls needed! Even if your page contains many database calls and complex logic, it will be executed once and cached for period you specify. No more overload!

This is a very efficient and simple method, to cache your most visited dynamic pages. [Tmawto.com](https://www.tmawto.com) website is built on PageCache, and is very fast.

Why another PHP Caching class?
----
It is not intended for everyone, only for those who wants to include a couple lines of code on top of their dynamic PHP pages and be able to cache them fully. No worrying about cache file name setup for each URL, no worries about your dynamically generated URL parameters and changing URLs. PageCache detects those changed and caches accordingly.

Lots of caching solutions focus on keyword-based approach, where you need to setup a keyword for your content (be it a full page cache, or a variable, etc.). There are great packages for keyword based approach. One could also use a more complex solution like a cache proxy, Varnish. PageCache on the other hand is a simple full page only caching solution, that does exactly what its name says - generates page cache in PHP.   

How PageCache works
----
It uses various strategies to differentiate among separate versions of the same page. 

PageCache doesn't ask you for a keyword, it automatically generates them based on Strategies. You can define your own naming strategy, for example to incorporate logged in users into your applications. In this situations, URL might remain same, while content of the page will be different for each logged in user.


```php
<?php
require_once __DIR__.'/../vendor/autoload.php';

$cache = new PageCache\PageCache();
$cache->init();

//rest of your PHP page code, everything below will be cached
```

For more examples see code inside [PageCache examples](examples/) directory.

For those who wonder, cache is saved into path specified in config file or using API, inside directories based on file hash. Based on the hash of the filename, 2 subdirectories will be created (if not created already), this is to avoid numerous files in a single cache directory. 


Caching Strategies
------------------
PageCache uses various strategies to differentiate among separate versions of the same page. 

`DefaultStrategy()` is the default behaviour of PageCache. It caches pages and generated cache filenames using this PHP code: `md5($_SERVER['REQUEST_URI'] . $_SERVER['SCRIPT_NAME'] . $_SERVER['QUERY_STRING'])`. You could create your own naming strategy and pass it to PageCache:

```php
$cache = new PageCache\PageCache();
$cache->setStrategy( new MyOwnStrategy() );
```

Included with the PageCache is the `MobileStrategy()` based on [Mobile_Detect](https://github.com/serbanghita/Mobile-Detect) . It is useful if you are serving the same URL differently accross devices. See [cache_mobiledetect.php PageCache example](examples/cache_mobiledetect.php) file for demo using MobileDetect._

You can define your own naming strategy, for example to incorporate logged in users into your applications. In this situations, URL might remain same, while content of the page will be different for each logged in user.

Config file
----
Although not required, configuration file can be specified during PageCache initialization for system wide caching properties

```php
//optional system-wide cache config
$config_file_ = __DIR__.'/config.php';
$cache = new PageCache\PageCache($config_file_);
```

All available configuration options from a config file:
```php
$config = array(

    //generated cache files less than this many bytes, are considered invalid and are regenerated
    'min_cache_file_size' => 2000,

    // set true to enable loging, not recommended for production use, only for debugging
    'enable_log' => false,

    //current page's cache expiration in seconds
    'expiration' => 20 * 60,

    //log file location, enable_log must be true for loging to work
    'log_file_path' => __DIR__ . '/log/cache.log',

    //cache directory location (mind the trailing slash "/")
    'cache_path' => __DIR__ . '/tmp/cache/'
);
```

API - PageCache class 
---------------------
The following are public methods of PageCache class that you could call from your application. Check out examples for code samples.

- init():void - initiate cache, this should be your last method to call on PageCache object.
- setStrategy(\PageCache\StrategyInterface):void - set cache file strategy. Built-in strategies are DefaultStrategy() and MobileStrategy(). Define your own if needed.
- clearPageCache():void - Clear cache for current page, if this page was cached before.
- getPageCache():bool - Return current page cache as a string or false on error, if this page was cached before.
- getFile():string - Get current page's cache file name.
- setPath(string):void - Location of cache files directory.
- setExpiration(int):void - Time in seconds for cache to expire.
- logFilePath(string):void - Set Log file path.
- enableLog():void - Enable logging.
- disableLog():void - Disable logging.
