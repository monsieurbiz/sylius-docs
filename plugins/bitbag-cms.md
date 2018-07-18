# Bitbag CMS plugin

This plugin makes it possible to create CMS content in Sylius.
The plugin is available on [Github](https://github.com/BitBagCommerce/SyliusCmsPlugin) and is installed using Composer.

# First installation

Their documentation can be found [here](https://github.com/BitBagCommerce/SyliusCmsPlugin/blob/master/doc/installation.md).
We're using version `2.0.`.

In order to install it you have to:
- run `composer install`.
- run `console doctrine:schema:update --force`.
- run `console sylius:theme:assets:install`.

If you had a previous version of the plugin you should replace step 2 above by:
- run `console doctrine:migrations:diff`.
- run `console doctrine:migrations:migrate`.

After that you can go in the admin panel and you'll see a new entry in the menu allowing to add CMS pages, blocks...

# Add CMS content

This can be done using admin panel or using [fixtures](/fixture/cms-fixture.md).
