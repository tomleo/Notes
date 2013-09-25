============
Django Notes
============

Debugging
=========

Error: cannot import name <Name>
--------------------------------

STR: When running manage.py runserver normally a traceback will be displayed
however, sometimes you get the cryptic error "cannot import name <name>"

Solution::
    
    python manage.py shell --traceback

It should also be noted that the problem can sometimes be in the middle of the
traceback not at the end. [1]_

.. [1] http://stackoverflow.com/a/8797685/465270


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
------

By adding `folllow=True` to your post request the client will add an attribute
redirect_chain to the response object::

    >> resp.redirect_chain
    [('http://testserver/submission/complete/', 302)]

The tuple contains the URL the view will redirect to and the status code which
should be a 302 redirect.

.. code-block:: python
    self.client.post(/species-transformation/, {'pet_id': i, 'species': 'Cat'})
    post_resp = client.post(post_url, payload, follow=True)
    self.assertEqual(post_resp.status_code, 200)
    #Build Absolute URL
    request = post_resp.request
    site_url = '%s://%s' % (request.get('wsgi.url_scheme'), request.get('SERVERNAME'))
    self.assertEqual(post_resp.redirect_chain[0][0], '{0}{1}'.format(site_url, '/species-transformation/complete/')
    self.assertEqual(post_resp.redirect_chain[0][1], 302)

The Shortcut
------------

.. code-block:: python

    post_resp = client.post(post_url, payload)
    self.assertRedirects(post_resp, '/species-transformation/complete/')


Django ORM
==========

pet_set is a lazy object that only makes a call to the database when you begin
to iterate over it. When the queryset is evaluated it will caches the results
so latter calls to pet_set will not also call the database.

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

via http://blog.etianen.com/blog/2013/06/08/django-querysets/

Forms
=====

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

Form Wizard
-----------

The form wizard will return a 404 after a schema migration due to what is
cached.

Solution::

    append the form wizard url with /?reset

