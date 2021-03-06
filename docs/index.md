# django-greeking

Django template tools for printing filler, a technique from the days of hot type known as greeking.


## Features

Greeking can:

* Generate filler images from [placehold.it](http://placehold.it), [placekitten.com](http://www.placekitten.com) and [Fill Murray](http://www.fillmurray.com/).
* Print pangrams in a variety of languages. A pangram is a phrase that includes every letter of an alphabet.
* Create ``Story``, ``Image``, ``RelatedItem`` and ``Quote`` objects with boilerplate text, URLs and a set of the attributes common to news.
* Print snippets from Lewis Carroll's poem [Jabberwocky](http://en.wikipedia.org/wiki/Jabberwocky).
* Import an list of filler comments for use in greeking [Django's 'contrib' comments app](http://docs.djangoproject.com/en/dev/ref/contrib/comments/).


## Installation

```bash
pip install greeking
```


## Getting started

Before you can use any of the template tags, you have to add the app to the
``INSTALLED_APPS`` your settings.py file, like so:

```python
INSTALLED_APPS = [
    ...
    'greeking',
]
```

And then import the library into your template.

```html+django
{% load greeking_tags %}
```

Then you just need to call out the tag you want to use.


## Placeholder images

All placeholder providers expect a width and height to be provided.

One provider we have a templatetag for is [placehold.it](https://placehold.it/).

```html+django
{% placeholdit 400 250 %}
```

<img src="https://placeholdit.imgix.net/~text?txtsize=38&txt=400%C3%97250&w=400&h=250">

Another is [placekitten.com](https://placekitten.com/)

```html+django
{% placekitten 200 200 %}
```

<img src="https://placekitten.com/200/200">


Another is [fillmurray.com](http://www.fillmurray.com/).

```html+django
{% fillmurray 600 200 %}
```

<img src="http://www.fillmurray.com/600/200">

### Customizing images

The ``placeholdit`` tag allows for the text over the image to be customized, as well as the text and background colors.

```html+django
{% placeholdit 400 250 text='Hello' %}
```

<img src="https://placeholdit.imgix.net/~text?txtsize=38&txt=Hello&w=400&h=250&txttrack=0">

```html+django
{% placeholdit 400 250 background_color='fff' text_color='000' %}
```

<img src="https://placeholdit.imgix.net/~text?txtsize=38&bg=ffffff&txtclr=000000&txt=400%C3%97250&w=400&h=250">

## Pangrams

A pangram is a phrase that includes every letter of an alphabet. It is useful when testing font implementations.

By default, an English pangram is returned.

```html+django
{% pangram %}
```

> The quick brown fox jumps over the lazy dog.

A set of other language are available, including Japanese, Spanish, French and German. They can be called by providing
the language code like so:

```html+django
{% pangram 'de' %}
```

> Falsches Üben von Xylophonmusik quält jeden größeren Zwerg.

Here is the complete list of available languages.

|Code|Language|
|:---|:-------|
|en|English|
|da|Danish|
|de|German|
|el|Greek|
|es|Spanish|
|fr|French|
|ga|Irish|
|he|Hebrew|
|hu|Hungarian|
|is|Icelandic|
|jp|Japanese|
|pl|Polish|
|ru|Russian|
|tr|Turkish|


## L.A. Times ipsum

A set of objects with boilerplate text, URLs and other attributes common to news. Used by the Los Angeles Times Data Desk to greek
its documentation and pages under development.

The library can generate ``Story``, ``Image``, ``RelatedItem`` and ``Quote`` objects.

They should be assigned to variables in the template and then printed out as needed.

### Story objects

```html+django
{% latimes_story as obj %}

<h1>{{ obj.headline }}</h1>
<div>{{ obj.byline }}</div>
```

Which would print out as:

> <h1>This is not a headline</h1>
<div>This is not a byline</div>

Here are all the attributes on a ``Story`` object:

|Name|
|:---|
|slug|
|headline|
|byline|
|pub_date|
|canonical_url|
|kicker|
|description|
|sources|
|credits|
|content|
|image|


### Image objects

```html+django
{% latimes_image 250 250 as obj %}

<img src="{{ obj.url }}">
<p>Credits: {{ obj.caption }}. ({{ obj.credit }})</p>
```

Which would print out as:

> <img src="https://placeholdit.imgix.net/~text?txtsize=23&bg=cccccc&txt=250%C3%97250&w=250&h=250">
<p>Credits: This is not an image caption. (This is not an image credit)</p>

You give images a custom background color like so:

```html
{% latimes_image 250 250 000000 as obj %}
```

Which would give the image a background color of ```#000000```. If there is no background color set, it will automatically be ```#CCCCCC```.


Here are all the attributes on an ``Image`` object:

|Name|
|:---|
|url|
|credit|
|caption|


### Quote objects

```html+django
{% latimes_quote as obj %}

<q>{{ obj.quote }}</q>
<p> — {{ obj.source }}</p>
```

Which would print out as:

> <q>This is not a quote</q>
<p> — This is not a source</p>


Here are all the attributes on a ``Quote`` object:

|Name|
|:---|
|quote|
|source|


### Related item lists

Related items link to other similar stories at the bottom of an article. By default, there are 4 related items.

```html+django
{% latimes_related_items as related_items %}

{% for item in related_items %}
    <div>
        <a href="{{ item.url }}">
           <p>{{ item.headline }}</p>
        </a>
        <a href="{{ item.url }}">
           <img src="{{ item.image.url }}">
        </a>
    </div>
{% endfor %}
```

Here are all the attributes on a `Related` object:

|Name|
|:---|
|headline|
|url|
|image|


## Jabberwocky

["Jabberywocky"](https://en.wikipedia.org/wiki/Jabberwocky) is a 1871 poem by Lewis Carroll, the author of "Alice in Wonderland." Selections can be printed by using the tag below.
The number of paragraphs can be optionally provided to limit its length. The poem has seven paragraphs in total.

```html+django
{% jabberwocky 3 %}
```

> 'Twas brillig, and the slithy toves
Did gyre and gimble in the wabe;
All mimsy were the borogoves,
And the mome raths outgrabe.

> "Beware the Jabberwock, my son!
The jaws that bite, the claws that catch!
Beware the Jubjub bird, and shun
The frumious Bandersnatch!"

> He took his vorpal sword in hand:
Long time the manxome foe he sought-
So rested he by the Tumtum tree,
And stood awhile in thought.


## Comments

An object_list of filler comments for use in greeking content for Django's [popular comments application](https://github.com/django/django-contrib-comments).

```html+django
{% greek_comment_list as comment_list %}
{% for comment in comment_list %}
    <div id="c{{ comment.id }}">
            <p>{{ comment.comment }}</p>
            <p>{{ comment.user_name }}</p>
            <p>{{ comment.submit_date|date:"F j, Y" }}</p>
            <p><a href="mailto:{{ comment.user_email }}">{{ comment.user_email }}</a></p>
            <p><a href="{{ comment.user_url }}">{{ comment.user_url }}</a></p>
    </div>
{% endfor %}
```

## Changelog

### v2.2.0 (Dec. 2016)

* Added ``latimes_ipsum`` library and documentation
* Removed the ``{% lorem %}`` tag since it is now packaged with the Django
* Removed broken image service lorem pixum
* Increased documentation of all libraries
* Added Turkish pangram


## Credits

* Pangrams are drawn from [Markus Kuhn](http://www.cl.cam.ac.uk/~mgk25/ucs/examples/quickbrown.txt).
* Comments drawn from the work of giants of our time.


## Open-source resources

* Repo: [https://github.com/datadesk/django-greeking](https://github.com/datadesk/django-greeking)
* Issues: [https://github.com/datadesk/django-greeking/issues](https://github.com/datadesk/django-greeking/issues)
* Packaging: [https://pypi.python.org/pypi/greeking](https://pypi.python.org/pypi/greeking)
* Testing: [https://travis-ci.org/datadesk/django-greeking](https://travis-ci.org/datadesk/django-greeking)
* Coverage: [https://coveralls.io/r/datadesk/django-greeking](https://coveralls.io/r/datadesk/django-greeking)
