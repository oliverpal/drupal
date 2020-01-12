# Drupal 8 cheatsheet

## Drupal 8

### Table of contents
1. [Installation with composer](#installation-with-composer)
2. [Usage of composer](#usage-of-composer)
3. [Local Setup for development](#local-dev-setup)
4. [Environment variables](#env-variables)
5. [Drush](#drush)

### <a name="installation-with-composer">Installation with composer</a>

There are differents methods of installing drupal: direct download, with drush or with composer.
But installation with composer is the recommended one and we will stick to that, because composer also manages all the dependencies for modules etc. Thanks to composer.json we will also have a consistent deployment process.

Install Drupal latest core into my_site_name_dir

```
composer create-project drupal/recommended-project my_site_name_dir --no-interaction
```

### <a name="usage-of-composer">Usage of composer</a>

Check if there are any updates

```
composer outdated 'drupal/*'
```

Install a module

```
composer require drupal/module_name
```

Update a module

```
composer update drupal/module_name --with-dependencies
```

Remove a module

```
composer remove drupal/module_name
```

### <a name="local-dev-setup">Local Setup for development</a>

For local development we definde local settings for a good dev workflow.
These settings include:

* Disabling caching
* Enable Twig Debugging in browser console
s
1. Define a local settings php
```
cp /sites/example.settings.local.php  /sites/default/settings.local.php
```

2. Uncomment the following lines in settings.php
```
if (file_exists($app_root . '/' . $site_path . '/settings.local.php')) {
  include $app_root . '/' . $site_path . '/settings.local.php';
}
```

3. Uncomment the following lines in settings.local.php
```
$settings['container_yamls'][] = DRUPAL_ROOT . '/sites/development.services.yml';
```

4. In /sites/default/development.services.yml add complete parameters
```
parameters:
  http.response.debug_cacheability_headers: true
  twig:config
    debug:true
    auto_reload: true
    cache:false
```
### <a name="env-variables">Environment variables</a>

It is recommended to leave sensitive access informations like for databases etc. also in environment variables outside the document root.
So database settings we will set in .env on webserver.

In composer root of drupal project (not web directory) on your remote webserver

```
cp .env.example .env
```
and write your database settings to the .env file.  This is also good practice on local dev server so all settings.php will have same definition.

```
//.env
MYSQL_DATABASE=database_name
MYSQL_HOSTNAME=localhost
MYSQL_PASSWORD=db_pass
MYSQL_PORT=3306
MYSQL_USER=db_user

//settings.php
$databases['default']['default'] = [
'database' => getenv('MYSQL_DATABASE'),
  driver' => 'mysql',
  host' => getenv('MYSQL_HOSTNAME'),
  namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
  password' => getenv('MYSQL_PASSWORD'),
  port' => getenv('MYSQL_PORT'),
  prefix' => '',
  username' => getenv('MYSQL_USER'),
];
```

### <a name="drush">Drush</a>

[Drush](https://www.drush.org/) is a very convenient command line interface (cli) for drupal.
It comes automatically when you are installing drupal with composer.

### Single Site Installation
```
// Check site status
drush status

// Cache clear/rebuild
drush cr

// Database update
drush updb

// Import configuration
drush cim -y

// Export configuration
drush cex -y
```

### Multisite Installation
```
// Check site status
drush -l mysite.dev status

// Cache clear/rebuild
drush -l mysite.dev cr

// Database update
drush -l mysite.dev updb

// Import configuration
drush -l mysite.dev cim -y

// Export configuration
drush -l mysite.dev cex -y
```

