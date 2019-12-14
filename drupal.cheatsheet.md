# Drupal cheatsheet for development purposes


## Drupal 8

### Table of contents
1. [Installation with composer](#installation-with-composer)
2. [Usage of composer](#usage-of-composer)
2. [Local Setup for development](#local-dev-setup)

### <a name="installation-with-composer">Installation with composer</a>

Install Drupal latest core into my_drupal_project

```
composer create-project drupal/recommended-project my_site_name_dir --no-interaction
```

### <a name="usage-of-composer">Usage of composer</a>

@todo

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
