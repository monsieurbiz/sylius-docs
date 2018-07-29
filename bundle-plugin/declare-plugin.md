# Declare a new plugin

A Sylius plugin is a Symfony bundle which extends the e-commerce framework's functionalities.

Let's create a MbizCustomPlugin in our Sylius installation !

First create the file `src/MbizCustomPlugin/MbizCustomPlugin.php` :

```php
<?php

declare(strict_types=1);

namespace Mbiz\MbizCustomPlugin;

use Sylius\Bundle\CoreBundle\Application\SyliusPluginTrait;
use Symfony\Component\HttpKernel\Bundle\Bundle;

final class MbizCustomPlugin extends Bundle
{
    use SyliusPluginTrait;
}
```

Then you have to register your plugin in `app/AppKernel.php` : 

```php
public function registerBundles(): array
    {
        $bundles = [
            // [...]
            new \Mbiz\MbizCustomPlugin\MbizCustomPlugin(),
        ];
        // [...]
    }

```

Now, if you look in the `Sylius\Bundle\CoreBundle\Application\SyliusPluginTrait` class, you'll see the following function  : 
```php
<?php
    // [...]
    /**
     * Returns the plugin's container extension class.
     *
     * @return string
     */
    protected function getContainerExtensionClass(): string
    {
        $basename = preg_replace('/Plugin$/', '', $this->getName());

        return $this->getNamespace() . '\\DependencyInjection\\' . $basename . 'Extension';
    }
```

So we have to create a new file in our plugin, in the `DependencyInjection` folder.

If you follow the function above, the filename will be `MbizCustomExtension.php`.

Create the file `src/MbizCustomPlugin/DependencyInjection/MbizCustomExtension.php` : 
```php
<?php

declare(strict_types=1);

namespace Mbiz\MbizCustomPlugin\DependencyInjection;

use Symfony\Component\DependencyInjection\ContainerBuilder;
use Symfony\Component\DependencyInjection\Extension\Extension;

final class MbizCustomExtension extends Extension
{
    /**
     * {@inheritdoc}
     */
    public function load(array $config, ContainerBuilder $container): void
    {
    }
}
```

This is not enough to make your bundle works, you'll have an error like : 
```bash
Fatal error: Uncaught Error: Class 'Mbiz\MbizCustomPlugin\MbizCustomPlugin' not found in /var/www/apps/sylius/app/AppKernel.php:37
Stack trace:
#0 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php(409): AppKernel->registerBundles()
#1 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php(120): Symfony\Component\HttpKernel\Kernel->initializeBundles()
#2 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Bundle/FrameworkBundle/Console/Application.php(65): Symfony\Component\HttpKernel\Kernel->boot()
#3 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/Console/Application.php(145): Symfony\Bundle\FrameworkBundle\Console\Application->doRun(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))
#4 /var/www/apps/sylius/bin/console(29): Symfony\Component\Console\Application->run(Object(Symfony\Component\Console\Input\ArgvInput))
#5 {main}
  thrown in /var/www/apps/sylius/app/AppKernel.php on line 37
```

You have to add it in composer autoload in editing `composer.json` :
```json
{
    //...
    "autoload": {
        "psr-4": {
			//...
             "Mbiz\\MbizCustomPlugin\\": "src/MbizCustomPlugin/"
        },
	//...
    },
}
```

Then, you have to update the autoload in composer.  
You can make it with the command `composer dump-autoload` in the root folder of your Sylius installation.

Now, your plugin is created, and you can begin to customize your Sylius platform with it :)

