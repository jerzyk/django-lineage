Django Lineage
==============

Handling which navigation element is active in a particular template can get a
bit ugly. Often, creating template blocks to drop an  "active" class on a
particular tab or list element.

Lineage makes things neater by defining your conditions once, and it
looks like this:

    {% load lineage %}

    <ul>
        <li {% ifanscestor 'home' %}class="active"{% endifanscestor %}">
            <a href="{% url 'home' %}">Home</a>
        </li>
        <li {% ifanscestor 'blog' %}class="active"{% endifanscestor %}">
            <a href="{% url 'blog' %}">Blog</a>
        </li>
        <li {% ifanscestor 'about' %}class="active"{% endifanscestor %}">
            <a href="{% url 'about' %}">About</a>
        </li>
    </ul>

Whereas before we might have had the following:

`base.html` template:

    <ul>
        <li class="{% block home_class %}{% endblock %}">
            <a href="{% url 'home' %}">Home</a>
        </li>
        <li class="{% block blog_class %}{% endblock %}">
            <a href="{% url 'blog' %}">Blog</a>
        </li>
        <li class="{% block about_class %}{% endblock %}">
            <a href="{% url 'about' %}">About</a>
        </li>
    </ul>

`home.html` template:

    {% extends 'base.html' %}
    {% block home_class %}active{% endblock %}

`blog.html` template:

    {% extends 'base.html' %}
    {% block blog_class %}active{% endblock %}

`about.html` template:

    {% extends 'base.html' %}
    {% block about_class %}active{% endblock %}

Installation
------------

Install using pip:

    pip install django-lineage

Add to INSTALLED_APPS in your Django settings file:

    INSTALLED_APPS = (
        'lineage',
    )

Usage
-----

First, load Lineage's template library:

    {% extends 'base.html'}
    {% load lineage %}

Lineage provides a single template tag `ifanscestor` which acts much like the
`if`/`endif` template tag:

    {% ifanscestor '/arbitrary/path/' %}
        This content is only displayed if the provided
        path matches the start of the current URL
    {% endifanscestor %}

`ifanscestor` can also handle a variable:

    {% ifanscestor some_variable %}{% endifanscestor %}

Or even the same arguments used by the `url` template tag to return an absolute
path given a view function or named pattern.

    {% ifanscestor 'core:model_detail' model.pk %}{% endifanscestor %}