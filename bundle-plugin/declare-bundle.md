# Declare a new bundle

Let's create a CustomBundle in our Sylius installation !

First create the file `src/CustomBundle/CustomBundle.php` :

```php
<?php

declare(strict_types=1);

namespace CustomBundle;

use Symfony\Component\HttpKernel\Bundle\Bundle;

class CustomBundle extends Bundle
{
}
```

Then you have to register your bundle in `app/AppKernel.php` : 

```php
public function registerBundles(): array
    {
        $bundles = [
            // [...]
            new \CustomBundle\CustomBundle(),
        ];
        // [...]
    }

```

This is not enough to make your bundle works, you'll have an error like : 
```bash
Fatal error: Uncaught Symfony\Component\Debug\Exception\ClassNotFoundException: Attempted to load class "CustomBundle" from namespace "CustomBundle".
Did you forget a "use" statement for another namespace? in /var/www/apps/sylius/app/AppKernel.php:33
Stack trace:
#0 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php(409): AppKernel->registerBundles()
#1 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php(120): Symfony\Component\HttpKernel\Kernel->initializeBundles()
#2 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Bundle/FrameworkBundle/Console/Application.php(65): Symfony\Component\HttpKernel\Kernel->boot()
#3 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/Console/Application.php(145): Symfony\Bundle\FrameworkBundle\Console\Application->doRun(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))
#4 /var/www/apps/sylius/bin/console(29): Symfony\Component\Console\App in /var/www/apps/sylius/app/AppKernel.php on line 33
```

You have to add it in composer autoload in editing `composer.json` :
```json
{
    //...
    "autoload": {
        "psr-4": {
			//...
            "CustomBundle\\": "src/CustomBundle/"
        },
	//...
    },
}
```

Then, you have to update the autoload in composer.  
You can make it with the command : `composer dump-autoload`

Now, your bundle is created, and you can begin to customize your Sylius platform with it :)

