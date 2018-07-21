# Extend a layout

In your twig templates, you will extend an existing layout to add only your custom content.

You can find many layouts inside the Sylius application : 
 - The basic : `{% extends '@SyliusShop/layout.html.twig' %}`
 - The account : `{% extends '@SyliusShop/Account/layout.html.twig' %}`
 - The account address book : `{% extends '@SyliusShop/Account/AddressBook/layout.html.twig' %}`
 - The checkout : `{% extends '@SyliusShop/Checkout/layout.html.twig' %}`

Let's take an example with the basic layout, we can put our content in `{% block content %}` and `{% endblock %}` :

```twig
{% extends '@SyliusShop/layout.html.twig' %}

{% block content %}
    <h2>My title</h2>
    <p>
        My content.
    </p>
{% endblock %}
```

Your new page will retrieve Sylius extended layout.