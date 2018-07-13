# Get Sylius design

If you create a new bundle with a new page, you may want to add a bit of design to it instead of getting a blank page.

In your template, add on the first line `{% extends '@SyliusShop/layout.html.twig' %}`

In order to add you own content you have to put it between `{% block content %}` and `{% endblock %}`

Example:

```
{% extends '@SyliusShop/layout.html.twig' %}

{% block content %}
    <h2>My title</h2>
    <p>
        My content.
    </p>
{% endblock %}
```

Your new page will retrieve Sylius default design.