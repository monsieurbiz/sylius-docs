# Create a simple fixture

We will create a simple fixture to add a custom category

Modify the file `apps/sylius/app/config/config.yml` : 
```yml
imports:
	[...]
    - { resource: '@AppBundle/Resources/config/app/config.yml' }
	[...]
```	

Create the file `AppBundle/Resources/config/app/config.yml'` :
```yml
imports:
    - { resource: "@AppBundle/Resources/config/app/fixtures.yml" }
    - "@AppBundle/Resources/config/services.xml"
```

Create the file `AppBundle/Resources/config/app/fixtures.yml` : 
```yml
sylius_fixtures:
    suites:
        default:
            fixtures:
                custom_taxon: ~
```

Create the file `AppBundle/Resources/config/services.xml` :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<container xmlns="http://symfony.com/schema/dic/services" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">
    <services>
        <service id="default.fixtures.custom_taxon" class="AppBundle\Fixture\CustomTaxonFixture">
            <argument type="service" id="sylius.fixture.taxon" />
            <tag name="sylius_fixtures.fixture" />
        </service>
    </services>
</container>
```

?> At this moment, you can flush the cache

```bash
console cache:clear --no-debug
```

Créer le fichier `AppBundle/Fixture/CustomTaxonFixture.php` : 
```php
<?php

namespace AppBundle\Fixture;

use Sylius\Bundle\FixturesBundle\Fixture\AbstractFixture;
use Sylius\Bundle\CoreBundle\Fixture\AbstractResourceFixture;

final class CustomTaxonFixture extends AbstractFixture
{
    private $taxonFixture;

    public function __construct(AbstractResourceFixture $taxonFixture)
    {
        $this->taxonFixture = $taxonFixture;
    }

    public function getName(): string
    {
        return 'custom_taxon';
    }

    public function load(array $options): void
    {
        $this->taxonFixture->load(['custom' => [[
            'code' => 'category',
            'name' => 'Category',
            'children' => [
                [
                    'code' => 'toto',
                    'name' => 'Toto',
                ],
            ],
        ]]]);

    }
}
```

You can see the `custom_taxon` fixture with the command `console sylius:fixtures:list`
To run the suite, launch the command `console sylius:fixtures:load`

?> During the fixtures load, all your data will be erased
