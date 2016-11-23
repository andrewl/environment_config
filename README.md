Environment Config
==================

This module allows you to override configuration on a per-environment basis.

Motivation
----------

In Drupal 8, settings are managed in one of two ways.
- The Configuration API is for "Configuration is a place to store information that you would want to synchronize from development to production." (https://www.drupal.org/docs/8/api/configuration-api/configuration-api-overview).
- The State API is for settings "specific to an individual environment."
  (https://www.drupal.org/docs/8/api/state-api)


In most cases this is fine, however it does rely on module developers knowing ahead of time whether your use will require a setting to be the same across all environments (ie stored with Configuration API) or differ across environments (ie stored with the State API). In some cases you'll want to manage a Configuration setting on a per-environment basis (eg use a different API key on different environments) - if so, this module is for you.

Usage
-----

1. Install the module

2. Create a yaml file containing the configuration settings that you wish to override inside a 'environment_config' key eg

<pre>
    environment_config:
      foo_module.settings
        api_key: ABCDE12345
      bar_module.settings
        api_key: 901112dfh330303
        some_other_setting: false
</pre>

3. Set the environment variable DRUPAL_CONFIG to the name of the file you created. (NB this can be outside of the document root).

4. When the cache is next rebuilt your new settings will be saved into the active configuration.

Notes
-----

This enables you to manage per-environment configurations in your infrastructure configuration management tool (eg ansible) by using that tool to maintain the yaml file and setting the DRUPAL_CONFIG environment variable to the correct value.

Todo
----

- Need to check for more use cases (eg setting complex objects).
- Currently only one file per environment is possible - won't work if eg dev and stage are on the same machine as they'll be sharing the DRUPAL_CONFIG environment variable.
