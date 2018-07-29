# Declare a new bundle

A bundle is created to extends Symfony behaviour, without using Sylius functionalities.

Let's create a MbizCustomBundle in our Sylius installation !

First create the file `src/MbizCustomBundle/MbizCustomBundle.php` :

```php
<?php

declare(strict_types=1);

namespace Mbiz\MbizCustomBundle;

use Symfony\Component\HttpKernel\Bundle\Bundle;

class MbizCustomBundle extends Bundle
{
}
```

Then you have to register your bundle in `app/AppKernel.php` : 

```php
public function registerBundles(): array
    {
        $bundles = [
            // [...]
            new Mbiz\MbizCustomBundle\MbizCustomBundle(),
        ];
        // [...]
    }

```

This is not enough to make your bundle works, you'll have an error like : 
```bash
Fatal error: Uncaught Error: Class 'Mbiz\MbizCustomBundle\MbizCustomBundle' not found in /var/www/apps/sylius/app/AppKernel.php:38
Stack trace:
#0 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php(409): AppKernel->registerBundles()
#1 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php(120): Symfony\Component\HttpKernel\Kernel->initializeBundles()
#2 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Bundle/FrameworkBundle/Console/Application.php(65): Symfony\Component\HttpKernel\Kernel->boot()
#3 /var/www/apps/sylius/vendor/symfony/symfony/src/Symfony/Component/Console/Application.php(145): Symfony\Bundle\FrameworkBundle\Console\Application->doRun(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))
#4 /var/www/apps/sylius/bin/console(29): Symfony\Component\Console\Application->run(Object(Symfony\Component\Console\Input\ArgvInput))
#5 {main}
  thrown in /var/www/apps/sylius/app/AppKernel.php on line 33
```

You have to add it in composer autoload in editing `composer.json` :
```json
{
    //...
    "autoload": {
        "psr-4": {
			//...
            "Mbiz\\MbizCustomBundle\\": "src/MbizCustomBundle/"
        },
	//...
    },
}
```

Then, you have to update the autoload in composer.  
You can make it with the command `composer dump-autoload` in the root folder of your Sylius installation.

Now, your bundle is created, and you can begin to customize your Sylius platform with it :)

