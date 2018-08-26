# Declare your entity 

## Define your entity

In our example we will take a custom entity named `Job`.
It contains these attributes :
- A code, to be identified
- An enabled flag
- A name
- A description
- A slug (for the URL)
- A channel association to choose on which channels we want to display it

So we have 2 specific features in this entity : 
- Name, description and slug are translatable
- Channel is a Many to Many association

We will also save creation date and last update date.

## Create YML files

So first, we will create the `Job` entity which will note contains translatable attributes : 

```yml
#AppBundle/Resources/config/doctrine/Job.orm.yml
AppBundle\Entity\Job:
    type: mappedSuperclass
    table: app_job
    id:
        id:
            type: integer
            id: true
            generator:
                strategy: AUTO
    fields:
        code:
            column: code
            type: string
            length: 64
            unique: true
        enabled:
            column: enabled
            type: boolean
        createdAt:
            column: created_at
            type: datetime
            nullable: true
            gedmo:
                timestampable:
                    on: create
        updatedAt:
            column: updated_at
            type: datetime
            nullable: true
            gedmo:
                timestampable:
                    on: update
    manyToMany:
        channels:
            targetEntity: Sylius\Component\Channel\Model\ChannelInterface
            joinTable:
                name: app_job_channels
                joinColumns:
                    job_id:
                        referencedColumnName: id
                inverseJoinColumns:
                    channel_id:
                        referencedColumnName: id
                        onDelete: CASCADE
```

And then, we can declare our `JobTranslation` entity : 
```yml
#AppBundle/Resources/config/doctrine/JobTranslation.orm.yml
AppBundle\Entity\JobTranslation:
    type: mappedSuperclass
    table: app_job_translation
    id:
        id:
            type: integer
            id: true
            generator:
                strategy: AUTO
    fields:
        slug:
            column: slug
            type: string
            nullable: true
        name:
            column: name
            type: string
            nullable: true
        description:
            column: description
            type: text
            nullable: true

```

At this moment, if you try a `console doctrine:migrations:diff`, you will have the error : 
```
Class 'AppBundle\Entity\Job' does not exist
```

So [let's create it](/create-new-entity/create-entity-classes.md) !