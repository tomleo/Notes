============
Django Notes
============

.. contents:: Topics
    :depth: 2

Debugging
=========

Unknown fields error in model form
----------------------------------

.. code-block:: python

    #models.py
    class Bacon(models.model):
        brand = models.CharField(max_length=64)
        date_created = models.DateTimeField(blank=True, null=True, auto_now_add=True)
        last_modified = models.DateTimeField(blank=True, null=True, auto_now=True)

    #forms.py
    class EditBacon(models.model):
        class Meta:
            model = Bacon
            fields = ('brand', 'date_created', 'last_modified')

When you attempt to `GET` a view you will get the following error::

    Unknown field(s) (date_created, last_modified) specified for Bacon

Why is this? I belive it has to do with `auto_now` and `auto_now_add` fields.
TODO: what happends if I move the timestamp functionality to the save() method?
TODO: what happends if I use exclude instead of include
TODO: what happends if I do not explitly define which fields to show (then
which fields show up?)

Remove static .pyc files
------------------------

In the base directory of your django project::

    find ./ -name \*.pyc -ls
    find ./ -name \*.pyc -exec rm {} \;
    find ./ -name \*.pyc -ls

Error: cannot import name <Name>
--------------------------------

STR: When running manage.py runserver normally a traceback will be displayed
however, sometimes you get the cryptic error "cannot import name <name>"

Solution::
    
    python manage.py shell --traceback

It should also be noted that the problem can sometimes be in the middle of the
traceback not at the end. [1]_

Delete Session Data
-------------------

Solution::
    
    from django.contrib.sessions.models import Session
    Session.objects.all().delete()


Testing
=======

Test Fixtures
-------------

Testing and Django by Carl Meyer (PyCon2012)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`YouTube <http://youtu.be/ickNQcNXiS4>`_.

Fixtures: Just say no::

    If you’ve got them in your code, burn them.
    16:30 - 23:30
    http://carljm.github.io/django-testing-slides/#22 to
    http://carljm.github.io/django-testing-slides/#27

Post Request Gotchas
--------------------

.. code-block:: python

    def setUp(self):
        self.pet_set = Pet.objects.filter(species="Dog")
        
    def test_that_pet_species_changed(self):
        for pet in Pet.objects.filter(species="Dog"):
            self.client.post(/species-transformation/, {'pet_id': i, 'species': 'Cat'})
            self.assertEqual(pet.species, 'Cat') # This will fail

        for pet in self.pet_set:
            self.client.post(/species-transformation/, {'pet_id': i, 'species': 'Cat'})
            self.assertEqual(pet.species, 'Cat') # This will fail

.. note::

    There are 2 hard problems in computer science: cache invalidation, naming
    things, and off-by-1 errors.

The reason these tests fail is because Django caches the result of the queryset
so while the post request will change the value `species` for each pet, the
assertion is comparing the older cached result.

Post Requests
-------------

Generally you redirect after a successful POST request in your view, so how
should you test this?

Method
~~~~~~

By adding `folllow=True` to your post request the client will add an attribute
redirect_chain to the response object::

    >> resp.redirect_chain
    [('http://testserver/submission/complete/', 302)]

The tuple contains the URL the view will redirect to and the status code which
should be a 302 redirect.

.. code-block:: python

    self.client.post('/species-transformation/', {'pet_id': i, 'species': 'Cat'})
    post_resp = client.post(post_url, payload, follow=True)
    self.assertEqual(post_resp.status_code, 200)
    #Build Absolute URL
    request = post_resp.request
    site_url = '%s://%s' % (request.get('wsgi.url_scheme'), request.get('SERVERNAME'))
    self.assertEqual(post_resp.redirect_chain[0][0], '{0}{1}'.format(site_url, '/species-transformation/complete/')
    self.assertEqual(post_resp.redirect_chain[0][1], 302)

The Shortcut
~~~~~~~~~~~~

.. code-block:: python

    post_resp = client.post(post_url, payload)
    self.assertRedirects(post_resp, '/species-transformation/complete/')


Unicode
=======

A Code Point
------------

Each letter corresponds to a code point::

    U+0639: code point
    U: Unicode
    0639: hex numbers

Unicode Byte order mark
-----------------------

When a series of code points are encoded they can sometimes start with `FE FF`
or `FF FE`. This is called a Unicode Byte order mark specifying high/low
endian. These marks will usually show up in UCS-2 type encodings which store 
chars in two bytes.

UTF-8
-----

Code points 0-127 are stored in a single byte, while code points above can be
take up as much as 6 bytes. By storing the first 128 codes in a single byte has
the nice side effect of making the unicode strings look the same as ascii.

Python
------

**Always put Unicode in unit tests**
Useful python functions, not encoding in unicode is better than ascii [2]_::

    >> i_uni = u'Ivan Krstić'
    >> i_uni
    u'Ivan Krsti\u0107'
    >> type(i_uni)
    unicode
    >> len(i_uni)
    11

    >> i_str = i_uni.encode('utf8')
    >> i_str
    'Ivan Krsti\xc4\x87'
    >> type(i_str)
    str
    >> len(i_str)
    12
    >> i_str.decode('utf8')
    u'Ivan Krsti\u0107'


Django ORM
==========

pet_set is a lazy object that only makes a call to the database when you begin
to iterate over it. When the queryset is evaluated it will caches the results
so latter calls to pet_set will not also call the database. [3]_

.. code-block:: python

    pet_set = Pet.objects.filter(species="Dog")
    # The query is executed and cached.
    for pet in pet_set:
        print(pet.first_name)
    # The cache is used for subsequent iteration.
    for pet in pet_set:
        print(pet.last_name)

.. note:: Caching can sometimes give you incorrect data
    See post request gotchas

The "if" statement will evaluate the queryset and cache the results so that
only one call to the database is made. 

You can avoid evaluating and potentially
caching a largy queryset by calling queryset.exists() which just checks if at
least one row in the database matches.

.. code-block:: python

    restaurant_set = Restaurant.objects.filter(cuisine="Indian")
    if restaurant_set.exists():
        print("Found some resturaunts!")

    # The `if` statement evaluates the queryset.
    if restaurant_set:
        # The cache is used for subsequent iteration.
        for restaurant in restaurant_set:
            print(restaurant.name)

you can evaluate a queryset without caching the results by calling iterator()

.. code-block:: python
    pet_set = Pet.objects.all()
    for pet in pet_set.iterator():
        print(pet.name)

iterate over large dataset example

.. code-block:: python

    pet_set = Pet.objects.all()
    pet_iterator = pet_set.iterator()
    #Look at first item in the iterator
    try:
        pet_one = next(pet_iterator)
    except:
        #No rows found, so no pets in the set
        pass
    else:
    from itertools import chain
        for pet in chain([pet_one], pet_set):
            print(pet.name)


QuerySet.update()
-----------------

- tries to make requested change in a single SQL UPDATE query instead of
  updating each row individually

.. Note:: 

    Doesn't execute custom save() methods on the model, 
    Doesn't trigger pre_save and post_save signals

QuerySet.delete()
-----------------

- tries to do a single SQL DELETE query

.. Note::

    Doesn't execute custom delete() methods on model
    Does sent pre_delete and post_delete signals (including for things deleted
    by cascade)


N+1 Database problem
--------------------

.. code-block:: python

    #models.py
    class Chef(models.Model):
        name = modles.CharField(max_length=64)

    class Restaurant(models.model):

        style_choices = (('chinese', 'Chinese'),)

        name = models.CharField(max_length=64)
        chef = models.ForeignKey(Chef)
        style = models.CharField(choices=style_choices)


The N+1 problem is essentially it takes (1 query to get the list of
restaurants), then you must iterate the result to find a matching chef for each
restaurant (N restaurants).


select_related()
````````````````

At sql level figures out the joins to get not just Restaurants but also
chef's related to Restaurants. (joining in SQL)

.. Note::

    Generic relations and m2m relations will not work

prefetch_related()
``````````````````

Can fetch m2m and generic relations with one query per relation (joining in
Python)


select_for_update()
-------------------

locks the selected rows until end of transaction

Custom QuerySet's
-----------------

.. code-block:: python

    class RestaurantStyleQuerySet(models.QuerySet):

        def chinese(self):
            return self.filter(style=self.model.style_choices.chinese)

        def itallian(self):
            return self.filter(style=self.model.style_choices.itallian)

    class RestaurantManager(models.Manager):
        def get_queryset(self):
            return RestaurantStyleQuerySet(self.model)

    chinese_food_restaurants = Restaurant.objects.all().chinese()


order_by 
--------

descending order
````````````````

high to low

.. code-block:: python

    >> User.objects.all().order_by('-id')[0].id
    44

ascending order
```````````````

low to high

.. code-block:: python

    >> User.objects.all().order_by('id')[0].id
    1


django.db.models.loading
------------------------

get_model(app_label, model_name) #Returns a model class
get_app(app_label) #Returns applications models module

.. code-block:: python

    >> from django.db.models.loading import get_model
    >> get_model('auth', 'user')
    django.contrib.auth.models.User

    >> from django.db.models.loading import get_app
    >> get_app('auth')
    <module 'django.contrib.auth.models' from '/usr/local/lib/python2.7/site-packages/django/contrib/auth/models.pyc'>

    >> from django.db.models.loading import get_models
    >> get_models()

generic relations
-----------------

django.contrib.contenttypes.generic

Allows you to create a foreign key that doesn't specify what model it relates
to. Can relate to any instance of any model.

Add ForeignKey to django.contrib.contenttypes.models.ContentType
Add Field that can hold a primary-key value (usually IntegerField or TextField)
Add GenericForeignKey specifying FK and Primary-Key fields as arguments

.. code-block:: python

    class Tag(models.Model):
        content_type = models.ForeignKey(ContentType)
        object_id = models.TextField()
        object = generic.GenericForeignKey('content_type', 'object_id')
        tag = models.CharField(max_length=255)

    >> guido = User.objects.get(username='guido')
    >> t = Tag(object=guido, tag='bdfl')
    >> t.save()
    >> t.object
    <User: guido>


DatabaseIntrospection
---------------------

- Knows how to get lists of tables, columns, relationships
- Knows how to map DB's internal column types back to Django field types

inspectdb

    management command that will look at your database and reverse engineer
    Django models for it [5]_

dbshell

    management command that will open up an iteractive shell/console session to
    the DB

SQLCompiler
-----------

- Turns Django Query instance into SQL
- Query.get_compiler() returns a SQLCompiler instance for that Query [6]_


Forms
=====

Figure out how to set all Django form fields via JS
Figure out how to create forms using form field variables

.. code-block:: python

    #Example form that will be referenced more in this section
    class cars(forms.ModelForm):
        class Meta:
            fields = ('make', 'model', 'year')


Generating Form Fields
----------------------

For more information see the docs [4]_ ::

    {{ form.make.label_tag() }}   ->   <label for="id_make">Installing Solar PV since</label>
    {{ form.make }}               ->   <input type="text" name="make" value="ford" id="id_make">
    {{ form.make.value }}         ->   ford



Get Form Field Id
-----------------

TODO: figure out how to do this
http://stackoverflow.com/questions/3763423/how-to-get-form-fields-id-in-django


Image field / Logo field not showing up in post data
----------------------------------------------------

Their are a few possible reasons why image/file fields do not show up in POST data. But the most
common mistakes I make are (1) forgetting to add multipart/form-data to the form in the template
and (2) forgetting to pass request.FILE to the form.

.. code-block:: html

    <form class="form-horizontal" action="" method="post" enctype="multipart/form-data" accept-charset="utf-8">

Model Forms with m2m Data
-------------------------

If a m2m field exists in the form, but is not rendered in the template then all
m2m data for a given instance will be lost.
For example if you fill out a *DogForm* then add toys via the *DogToyForm* then
go back and edit *DogForm* then all his toys will be lost!

.. code-block:: python

    #in models.py
    class DogToy(models.Model):
        name = models.CharField(max_length=60, blank=True)

    class Dog(models.Model):
        color = models.CharField(max_length=60, blank=True)
        toys = models.ManyToManyField('DogToy', blank=True)

    #in forms.py
    class DogForm(forms.ModelForm):
        class Meta:
            model = Dog

    class DogToyForm(forms.ModelForm):
        class Meta:
            model = DogToys

    #in template
    <form action="." method="post">
        {% csrf_token %}
        {{ form.color }}
        <input type="submit" />
    </form>

So if you wish to allow a dogs information to be edited in a form that does not
include photos it is import that you exclude forms that will not be rendered in
the template. In the example above the *DogToyForm* should be changed to the
following

.. code-block:: python

    class DogToyForm(forms.ModelForm):
        class Meta:
            model = DogToys
            exclude = ('toys')

Model Mixins
------------

Lets say you want to associate a score with your dog toy, this can be achived
like this

.. code-block:: python

    class Score(models.Model):
        score = models.PositiveIntegerField()
        dog = models.ForeignKey(Dog, help_text="The dog's score")

    class DogToy(Score):
        name = models.CharField(max_length=60)



Form Wizard
-----------

The form wizard will return a 404 after a schema migration due to what is
cached.

Solution::

    append the form wizard url with /?reset


Remove Default value from ModelChoiceField
------------------------------------------

Lets say you have the following bacon types ['maple', 'smoked', uncured'] when
the ModelChoiceField will use a Select widget. The problem is that the HTML
returned is::

    <select>
        <option value="" selected="selected">---------</option>
        <option value="1">Maple</option>
        <option value="2">Smoked</option>
        <option value="2">Uncured</option>
    </select>

To fix this add the empty_label=None option to the field.

.. code-block:: python

    class BaconTypesForm(forms.ModelForm):
        class Meta:
            model = BaconTypes
            fields = ('bacon_types',)

        def __init__(self, *args, **kwargs):
            super(BaconTypesForm, self).__init__(*args, **kwargs)
            self.fields['bacon_type'].empty_label = None

    **


Nginx
=====

Nginx cannot serve resources if they are not given the correct permissions. For
servering static files the following tends to work well::

    sudo chown -R my-user:www-data /webdirectory
    sudo chmod -R 0755 /webdirectory
                

.. [1] http://stackoverflow.com/a/8797685/465270
.. [2] http://farmdev.com/talks/unicode/
.. [3] http://blog.etianen.com/blog/2013/06/08/django-querysets/
.. [4] https://docs.djangoproject.com/en/dev/ref/forms/api/#django.forms.BoundField.id_for_label
.. [5] https://youtu.be/tkwZ1jG3XgA?list=WL&t=624
.. [6] https://youtu.be/tkwZ1jG3XgA?list=WL&t=843


./media/django-in-depth-2015.pdf (Slide #)


Django View/Template
====================

Tooltips for Radio Buttons Items (Chocies Field)

.. code-block:: python

    from django.utils.safestring import mark_safe
    choices = (
        ('1', mark_safe(u'<em>HTML</em><span>Is being put</span> inside tooltip')),
        ('2', mark_safe(u'<p>Tooltip logic could go here</p>')
    )



