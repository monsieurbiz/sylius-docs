# Declare Sylius resource

## YAML Configuration

Before continue, be sure your `AppBundle` config is included in `app/config/config.yml`.
In my case, it is : 
```yml
    # [...]
    - { resource: '@AppBundle/Resources/config/app/config.yml' }
    # [...]
```

Then, in your `AppBundle` config file, add this line : 
```yml
imports:
    # [...]
  - { resource: "@AppBundle/Resources/config/resources.yml" }
    # [...]
```

And create the `resources.yml` file : 
```yml
# src/AppBundle/Resources/config/resources.yml
imports:
    # [...]
    - { resource: resources/job.yml }
    # [...]
```

## Declare resource

Now, in the `job.yml` we will declare our Resource : 
```yml
# src/AppBundle/Resources/config/resources/job.yml
sylius_resource:
    resources:
        app.job:
            driver: doctrine/orm
            classes:
                model: AppBundle\Entity\Job
            translation:
                classes:
                    model: AppBundle\Entity\JobTranslation

```

We tell to Sylius that our Job resource will :
- Use `doctine/orm` as driver
- `AppBundle\Entity\Job` as model
- `AppBundle\Entity\JobTranslation` as translation model

Now you can use the doctrine migration 🎉
```bash
console console doctrine:migrations:diff;
```

A new migration file is created, if not, try to clear doctrine's metadata cache : 
```bash
console doctrine:cache:clear-metadata
```

## Migration file

The file is in the folder `app/migrations/` and will contains the SQL queries to create our entity in database : 

```php
<?php declare(strict_types=1);

namespace Sylius\Migrations;

use Doctrine\DBAL\Schema\Schema;
use Doctrine\Migrations\AbstractMigration;

/**
 * Auto-generated Migration: Please modify to your needs!
 */
final class Version20180826154151 extends AbstractMigration
{
    public function up(Schema $schema) : void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->abortIf($this->connection->getDatabasePlatform()->getName() !== 'mysql', 'Migration can only be executed safely on \'mysql\'.');

        $this->addSql('CREATE TABLE app_job (id INT AUTO_INCREMENT NOT NULL, code VARCHAR(64) NOT NULL, enabled TINYINT(1) NOT NULL, created_at DATETIME DEFAULT NULL, updated_at DATETIME DEFAULT NULL, UNIQUE INDEX UNIQ_D5AA533F77153098 (code), PRIMARY KEY(id)) DEFAULT CHARACTER SET UTF8 COLLATE UTF8_unicode_ci ENGINE = InnoDB');
        $this->addSql('CREATE TABLE app_job_channels (job_id INT NOT NULL, channel_id INT NOT NULL, INDEX IDX_56DD5D4CBE04EA9 (job_id), INDEX IDX_56DD5D4C72F5A1AA (channel_id), PRIMARY KEY(job_id, channel_id)) DEFAULT CHARACTER SET UTF8 COLLATE UTF8_unicode_ci ENGINE = InnoDB');
        $this->addSql('CREATE TABLE app_job_translation (id INT AUTO_INCREMENT NOT NULL, translatable_id INT NOT NULL, slug VARCHAR(255) DEFAULT NULL, name VARCHAR(255) DEFAULT NULL, description LONGTEXT DEFAULT NULL, locale VARCHAR(255) NOT NULL, INDEX IDX_8DDF7BAF2C2AC5D3 (translatable_id), UNIQUE INDEX app_job_translation_uniq_trans (translatable_id, locale), PRIMARY KEY(id)) DEFAULT CHARACTER SET UTF8 COLLATE UTF8_unicode_ci ENGINE = InnoDB');
        $this->addSql('ALTER TABLE app_job_channels ADD CONSTRAINT FK_56DD5D4CBE04EA9 FOREIGN KEY (job_id) REFERENCES app_job (id)');
        $this->addSql('ALTER TABLE app_job_channels ADD CONSTRAINT FK_56DD5D4C72F5A1AA FOREIGN KEY (channel_id) REFERENCES sylius_channel (id) ON DELETE CASCADE');
        $this->addSql('ALTER TABLE app_job_translation ADD CONSTRAINT FK_8DDF7BAF2C2AC5D3 FOREIGN KEY (translatable_id) REFERENCES app_job (id) ON DELETE CASCADE');
    }

    public function down(Schema $schema) : void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->abortIf($this->connection->getDatabasePlatform()->getName() !== 'mysql', 'Migration can only be executed safely on \'mysql\'.');

        $this->addSql('ALTER TABLE app_job_channels DROP FOREIGN KEY FK_56DD5D4CBE04EA9');
        $this->addSql('ALTER TABLE app_job_translation DROP FOREIGN KEY FK_8DDF7BAF2C2AC5D3');
        $this->addSql('DROP TABLE app_job');
        $this->addSql('DROP TABLE app_job_channels');
        $this->addSql('DROP TABLE app_job_translation');
    }
}
```

If you check in your database, tables are not created yet. To create it, use : 
```bash
console doctrine:schema:update --force
```

