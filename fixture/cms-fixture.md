# Create a cms fixture

We will create a fixture to add content using the bitbag cms plugin.

## Create a cms page

Add the following content to `apps/sylius/src/AppBundle/Resources/config/app/fixture.yml` under `fixtures`.
```
                page:
                    options:
                        custom:
                            code_of_the_cms_page:
                                translations:
                                    locale:
                                        name: "Name of the CMS page"
                                        slug: "Slug of the CMS page"
                                        content: |
                                            CMS page content.
                                    locale:
                                        name: "Name of the CMS page"
                                        slug: "Slug of the CMS page"
                                        content: |
                                            CMS page content.
```

You can add as much `locale` as you want. If you want to add more than one page just add another `code_of_the_cms_page`
section.

You have to replace the fields:
- `code_of_the_cms_page`: this is the code of the CMS page, has to be unique.
- `locale`: this is the locale of the CMS page. You can add as many locale as you want.
- `Name of the CMS page`: this is the name of the CMS page.
- `Slug of the CMS page`: this is the slug of the CMS page.
- `CMS page content.`: this is the content of your CMS page.

## Create a cms block
Add the following content to `apps/sylius/src/AppBundle/Resources/config/app/fixture.yml` under `fixtures`.

```
                block:
                    options:
                        custom:
                            code_of_the_cms_block:
                                translations:
                                    locale:
                                        content: |
                                            CMS block content.
                                    locale:
                                        content: |
                                            CMS block content.
```

You can add as much `locale` as you want. If you want to add more than one block just add another `code_of_the_cms_block`
section.

You have to replace the fields:
- `code_of_the_cms_block`: this is the code of the CMS block, has to be unique.
- `locale`: this is the locale of the CMS block. You cann add as many locale as you want.
- `CMS block content.`: this is the content of your CMS block.