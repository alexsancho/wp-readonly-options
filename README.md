# WP Plugin: Readonly Options
[![Build Status](https://travis-ci.org/alexsancho/wp-readonly-options.svg?branch=master)](https://travis-ci.org/alexsancho/wp-readonly-options)

Have you ever wanted to set all your options as `define('PLUGIN_OPTION','some_value')` in `wp_config.php`?

Sometimes you need to use 3rd party plugins that only gives you those options in admin pages and GUI.
And now you can't use git to version control your settings as you would love to do.

This plugin helps you to set all your settings in your code and doesn't force you to go over to admin pages.

## How it works
It works by allowing you to force the results of `get_option()` to your predefined values.

This also adds tiny amount if javascript into admin pages so that it can set readonly attributes to your options: `<input readonly>`.
This makes it easier for the users to understand that these options can't be changed.

This only works in `php7.0` version or better since we use arrays when defining constants. We also like to use scalar type hintings. Sorry legacy projects :(

## Installation
Prefered installation is with composer:

```json
{
    "require": {
        "alexsancho/wp-readonly-options": "^v1.0.0"
    },
    "extra": {
        "installer-paths": {
          "web/app/mu-plugins/{$name}/": ["type:wordpress-muplugin"]
        }
    }
}
```

## Code Example

My options page looks so empty and lonely:

<img width="812" alt="Before" src="https://cloud.githubusercontent.com/assets/5691777/17637568/14e14110-60ed-11e6-867b-7f921d73fb02.png">

I'll look up the keys from `/wp-admin/options.php` and I see that they are `sm_bucket` and `sm_key_json`.

You can also see the names by using Google Chrome inspector. Key name is same as input element ID.

I can use those keys with `WP_READONLY_OPTIONS` and I add the following code to my `wp-config.php`:

**php7.0**
```php
define( 'WP_READONLY_OPTIONS', array(
    'sm_bucket' => 'my-bucket.example.com'
    'sm_key_json' => '{
      "type": "service_account",
      "project_id": "XXXXXXXXXXXXXXXXXXXXXXX",
      "private_key_id": "XXXXXXXXXXXXXXXXXXXX",
      "private_key": "-----BEGIN PRIVATE KEY-----\nXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
      -----END PRIVATE KEY-----
    }'
));
```

**php5.X**
```php
define( 'WP_READONLY_OPTIONS', serialize( array(
    'sm_bucket' => 'my-bucket.example.com'
    'sm_key_json' => '{
      "type": "service_account",
      "project_id": "XXXXXXXXXXXXXXXXXXXXXXX",
      "private_key_id": "XXXXXXXXXXXXXXXXXXXX",
      "private_key": "-----BEGIN PRIVATE KEY-----\nXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
      -----END PRIVATE KEY-----
    }'
)));
```


Afterwise I can see the values in readonly mode:

<img width="806" alt="After" src="https://cloud.githubusercontent.com/assets/5691777/17637575/1c282f42-60ed-11e6-8622-7cff2466578b.png">

## Configuration
Disables readonly attribute setter Javascript hack. It might be incompatible with something.
```php
define('WP_READONLY_OPTIONS_NO_JS',true);
```
