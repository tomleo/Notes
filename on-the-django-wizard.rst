The Wizards of Django
=====================

How the django.contrib.formtools wizard works
---------------------------------------------

.. TODO:: 

    talk about brack3t article (tim the enchanter)

.. TODO:: 

    quick tutorial on how the django wizard works and how to implement one in
    your own django project

Pros of Using the django.contrib.formtools wizard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 - Written cleanly using CBV and is easy to change and extend
 - Built in handling for multiple related forms 
 - Has level of abstraction for dealing with sessions and caching
 - All server side code
 - Works even if you don't have JS (major browser support)

Cons of Using the django.contrib.formtools wizard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 - Server side validation can make the wizard less flexible: The form must
   validate before you can move forward in the wizard. Jumping between steps in
   a wizard can be difficult.
 - Conditional step logic is weird
 - Server-side validation can seem less responsive
 - All computation is server-side
 - step helper logic it's more confusing than helpful
 - A lot of setup logic in urls.py
 - Requires a form on each step of the wizard (you can circumvent this by
   passing an empty form)

How I would re-write the django wizard
--------------------------------------

 - Python/Django backend logic would be used for final POST
 - No concept of next/previous step on the Django side, it doesn't matter what
   step you are on and what fields are valid because only on the final
   submission of the wizard will anything be validated server side.
 - Each steps "form" would be validated client side
 - Form data stored client side (via localstorage)

Pros of new wizard
~~~~~~~~~~~~~~~~~~

 - By allowing JS to handling changing of step there is less complicated POST
   and session logic maintained to determine which step a user is currently on.
 - By storing form data locally there is less logic around caching forms still
   in progress, again this relies less of Django's session storage.
 - Django forms handle validation, the wizard solves this problem by having a
   different form for each step of the wizard, and validating each form as your
   go. With JS level validation you only have to validate a section of a wizard
   (i.e. the part of the form being displayed/shown via JS). Also you can avoid
   extra POST's to the server when data is incorrect. Client side logic can
   also be more flexible when it comes to jumping between steps as the whole
   form isn't getting validated by Django until the final post.

Cons of new wizard
~~~~~~~~~~~~~~~~~~

 - User needs to have JS
 - Not DRY, validation is duplicated there is JS client level validation and
   server side validation. Would require a front-end JS program from testing.
 - Wizard is now written in two languages.
 - if conditional forms are dependent on the state of a user or something else
   on the server end then a RESTfull-API call would be necessary. So while you
   have a simpler wizard view, you must now implement RESTfull view functions
   that must serve content to the user.





