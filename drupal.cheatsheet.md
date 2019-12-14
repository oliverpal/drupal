# Drupal cheatsheet for development purposes


## Drupal 8

### Setup local dev environment

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
