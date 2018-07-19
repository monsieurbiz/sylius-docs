# Create a cms fixture

We will create a fixture to add content using the bitbag cms plugin.

## Create a cms page

Add the following content to `apps/sylius/src/AppBundle/Resources/config/app/fixture.yml` under `fixtures`.
```
                page:
                    options:
                        custom:
                            IDENTIFIER_OF_YOUR_PAGE:
                                translations:
                                    fr_FR:
                                        name: "Name of the CMS page"
                                        slug: "Slug of the CMS page"
                                        content: |
                                            CMS page content.
                                    en_US:
                                        name: "Name of the CMS page"
                                        slug: "Slug of the CMS page"
                                        content: |
                                            CMS page content.
```

You can add as much locale as you want. If you want to add more than one page just add another `IDENTIFIER_OF_YOUR_PAGE`
section.

You have to replace the fields:
- `IDENTIFIER_OF_YOUR_PAGE`: this is the identifier of the CMS page, has to be unique.
- `fr_FR` or `en_US`: this is the locale of the CMS page. You can add as many locale as you want. Just keep one 😘.
- `Name of the CMS page`: this is the name of the CMS page.
- `Slug of the CMS page`: this is the slug of the CMS page. Can be identical as the identifier, but you can localize it, which is better.
- `CMS page content.`: this is the content of your CMS page.

## Create a cms block
Add the following content to `apps/sylius/src/AppBundle/Resources/config/app/fixture.yml` under `fixtures`.

```
                block:
                    options:
                        custom:
                            IDENTIFIER_OF_YOUR_BLOCK:
                                translations:
                                    fr_FR:
                                        content: |
                                            CMS block content.
                                    en_US:
                                        content: |
                                            CMS block content.
```

You can add as much `locale` as you want. If you want to add more than one block just add another `IDENTIFIER_OF_YOUR_BLOCK` section.

You have to replace the fields:
- `IDENTIFIER_OF_YOUR_BLOCK`: this is the code of the CMS block, has to be unique.
- `fr_FR` or `en_US`: this is the locale of the CMS block. You cann add as many locale as you want. Just keep one 😘.
- `CMS block content.`: this is the content of your CMS block.