Environment Config
==================

This module allows you to override configuration on a per-environment basis.

Motivation
----------

In Drupal 8, settings are managed in one of two ways.
- The Configuration API is for "Configuration is a place to store information that you would want to synchronize from development to production." (https://www.drupal.org/docs/8/api/configuration-api/configuration-api-overview).
- The State API is for settings "specific to an individual environment."
  (https://www.drupal.org/docs/8/api/state-api)


In most cases this is fine, however it does rely on module developers knowing ahead of time whether your use of their module will require a setting to be the same across all environments (ie stored with Configuration API) or differ across environments (ie stored with the State API). In some cases you'll want to manage a Configuration setting on a per-environment basis (eg use a different API key on different environments) - if so, this module is for you.

This enables you to manage per-environment configurations in your infrastructure configuration management tool (eg ansible) by using that tool to maintain the yaml file and setting the DRUPAL_CONFIG_DIR environment variable to the correct value.

Usage
-----

1. Install the module

2. Create a directory to contain your environment-specific settings.

3. Set the environment variable DRUPAL_CONFIG_DIR to the name of the directory you created. (NB this can be outside of the document root).

4. Create configuration management yaml files containing only the configuration settings that you wish to override. eg to modify the api_key setting of the foo_bar.settings configuration create a file called 'foo_bar.settings.yml' containing the following:
<pre>
api_key: ABCDE12345
</pre>

5. When the cache is next rebuilt your new settings will be saved into the active configuration.

Notes
-----

*IF YOU EXPORT YOUR ACTIVE CONFIGURATION (eg by running drush cex) THEN YOUR ENVIRONMENT-SPECIFIC CONFIGURATION WILL BE EXPORTED.* This will not usually be a problem provided you have the same set of environment-specific configurations on each environment you deploy to (because the exported setting will be overridden by their environment-specific counterparts). 

Pro tip: Use drush (or console) to create your initial yaml files then edit as appropriate. eg drush cget foo_bar.settings > my/environment/config/dir/foo_bar.settings.yml

An alternative approach to using this module would be to modify your build/deploy scripts to call a command to set these manually (eg drush cset foo_module.settings api_key ABCDE12345)

Todo
----

- Need to check for more use cases (eg setting complex objects).
- Currently only one file per environment is possible - won't work if eg dev and stage are on the same machine as they'll be sharing the DRUPAL_CONFIG environment variable.
