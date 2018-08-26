# Create entity classes

## Create interfaces

Before creating our entities, we will create their interfaces : 

```php
<?php
// src/AppBundle/Entity/JobInterface.php
declare(strict_types=1);

namespace AppBundle\Entity;

use Sylius\Component\Channel\Model\ChannelsAwareInterface;
use Sylius\Component\Resource\Model\ResourceInterface;
use Sylius\Component\Resource\Model\TimestampableInterface;
use Sylius\Component\Resource\Model\ToggleableInterface;
use Sylius\Component\Resource\Model\TranslatableInterface;

interface JobInterface extends
    ResourceInterface,
    TranslatableInterface,
    ToggleableInterface,
    TimestampableInterface,
    ChannelsAwareInterface
{
    public function getCode(): ?string;

    public function setCode(?string $code): void;
    
    public function getSlug(): ?string;

    public function setSlug(?string $slug): void;

    public function getName(): ?string;

    public function setName(?string $name): void;

    public function getDescription(): ?string;

    public function setDescription(?string $description): void;
}
```

We extend some interfaces in order to avoid to declare already implemented functions.

`ResourceInterface` adds : 
- `public function getId();`

`TranslatableInterface` adds : 
- `public function getTranslations(): Collection;`
- `public function getTranslation(?string $locale = null): TranslationInterface`
- `public function hasTranslation(TranslationInterface $translation): bool;`
- `public function addTranslation(TranslationInterface $translation): void;`
- `public function removeTranslation(TranslationInterface $translation): void;`
- `public function setCurrentLocale(string $locale): void;`
- `public function setFallbackLocale(string $locale): void;`

`ToggleableInterface` adds : 
- `public function isEnabled()`
- `public function setEnabled(?bool $enabled): void;`
- `public function enable(): void;`
- `public function disable(): void;`

`TimestampableInterface` adds : 
- `public function getCreatedAt(): ?\DateTimeInterface;`
- `public function setCreatedAt(?\DateTimeInterface $createdAt);`
- `public function getUpdatedAt(): ?\DateTimeInterface;`
- `public function setUpdatedAt(?\DateTimeInterface $updatedAt);`

`ChannelsAwareInterface` adds : 
- `public function getChannels(): Collection;`
- `public function hasChannel(ChannelInterface $channel): bool;`
- `public function addChannel(ChannelInterface $channel): void;`
- `public function removeChannel(ChannelInterface $channel): void;`

As you can see, we also have some functions to get or set translatable attributes.
Now, we can create the translation interface : 

```php
<?php
// src/AppBundle/Entity/JobTranslationInterface.php
declare(strict_types=1);

namespace AppBundle\Entity;

use Sylius\Component\Resource\Model\ResourceInterface;
use Sylius\Component\Resource\Model\TranslationInterface;

interface JobTranslationInterface extends ResourceInterface, TranslationInterface
{
    public function getSlug(): ?string;

    public function setSlug(?string $slug): void;

    public function getName(): ?string;

    public function setName(?string $name): void;

    public function getDescription(): ?string;

    public function setDescription(?string $description): void;
}
```

`ResourceInterface` adds : 
- `public function getId();`

`TranslationInterface` adds : 
- `public function getTranslatable(): TranslatableInterface;`
- `public function setTranslatable(?TranslatableInterface $translatable): void;`
- `public function getLocale(): ?string;`
- `public function setLocale(?string $locale): void;`

## Create classes

Now, we can implement our interfaces with our classes \o/
Do you remember the functions added by Sylius' interfaces ? 
We will not have to create them. We will use PHP traits to include it in our class. 

```php
<?php
// src/AppBundle/Entity/Job.php
declare(strict_types=1);

namespace AppBundle\Entity;

use Sylius\Component\Resource\Model\TimestampableTrait;
use Sylius\Component\Resource\Model\ToggleableTrait;
use Sylius\Component\Resource\Model\TranslatableTrait;
use Sylius\Component\Resource\Model\TranslationInterface;
use BitBag\SyliusCmsPlugin\Entity\ChannelsAwareTrait;

class Job implements JobInterface
{
    use ToggleableTrait;
    use TimestampableTrait;
    use ChannelsAwareTrait;
    use TranslatableTrait {
        __construct as protected initializeTranslationsCollection;
    }

    /** @var int */
    protected $id;

    /** @var string */
    protected $code;

    public function __construct()
    {
        $this->initializeTranslationsCollection();
        $this->initializeChannelsCollection();

        $this->createdAt = new \DateTime();
    }


    /** @var string */
    protected $categories;

    public function getId(): ?int
    {
        return $this->id;
    }

    public function setId(?int $id): void
    {
        $this->id = $id;
    }

    public function getCode(): ?string
    {
        return $this->code;
    }

    public function setCode(?string $code): void
    {
        $this->code = $code;
    }

    public function getSlug(): ?string
    {
        return $this->getJobTranslation()->getSlug();
    }

    public function setSlug(?string $slug): void
    {
        $this->getJobTranslation()->setSlug($slug);
    }

    public function getName(): ?string
    {
        return $this->getJobTranslation()->getName();
    }

    public function setName(?string $name): void
    {
        $this->getJobTranslation()->setName($name);
    }

    public function getDescription(): ?string
    {
        return $this->getJobTranslation()->getDescription();
    }

    public function setDescription(?string $description): void
    {
        $this->getJobTranslation()->setDescription($description);
    }

    /**
     * @return JobTranslationInterface|TranslationInterface|null
     */
    protected function getJobTranslation(): JobTranslationInterface
    {
        return $this->getTranslation();
    }

    /**
     * Create resource translation model.
     *
     * @return TranslationInterface
     */
    protected function createTranslation(): TranslationInterface
    {
        return new JobTranslation();
    }
}
```

For all translation's attributes , we use the function `getJobTranslation` which use `getTranslation` of `TranslatableTrait`.
So we will create the `JobTranslation` class.
This class extends `AbstractTranslation` which adds `TranslationInterface` methods.

```php
<?php
// src/AppBundle/Entity/JobTranslation.php
declare(strict_types=1);

namespace AppBundle\Entity;

use Sylius\Component\Resource\Model\AbstractTranslation;

class JobTranslation extends AbstractTranslation implements JobTranslationInterface
{
    /** @var int */
    protected $id;

    /** @var string */
    protected $slug;

    /** @var string */
    protected $name;

    /** @var string */
    protected $description;

    public function getId(): int
    {
        return $this->id;
    }

    public function setId(int $id): void
    {
        $this->id = $id;
    }

    public function getSlug(): ?string
    {
        return $this->slug;
    }

    public function setSlug(?string $slug): void
    {
        $this->slug = $slug;
    }

    public function getName(): ?string
    {
        return $this->name;
    }

    public function setName(?string $name): void
    {
        $this->name = $name;
    }

    public function getDescription(): ?string
    {
        return $this->description;
    }

    public function setDescription(?string $description): void
    {
        $this->description = $description;
    }
}

```

You can try now your doctriner migration. And you will see...
```
No changes detected in your mapping information.
```

We didn't [declare our resource to Sylius](/create-new-entity/declare-sylius-resource.md) !