=====================
Django Best Practices
=====================

Template Naming Convension
==========================

Snippets and Includes
---------------------

The location of snippet folders should be dependent on how generic the snippet
is. So there could be a `/template/snippets , '/template/market/snippets/`, and
`/template/market/quote_solarpv/snippets`

Code Structure
==============

Post Merge Commit Hook for SASS
-------------------------------

::
    touch .git/hooks/post-merge
    echo "#\!/bin/sh\ncompass compile stylesheets -e production --force" >> .git/hooks/post-merge
    chmod u+x .git/hooks/post-merge

    #This will minify the outputted CSS
    compass compile <path_to_compass_project> -e production --force

Minify and Compile Code with Django App
---------------------------------------

https://github.com/jezdez/django_compressor

Minify and Compile Code with Grunt
----------------------------------

We are not currently doing this and probably should. The CSS files we maintain should be
converted to SASS so that they will be compilled into the same CSS file as the
other SASS files.

Minifying and Compiling Code could be done via Grunt Task Runner placed in es-site/es/es/static

.. code-block:: javascript

    //Package.json we could alternativly use bower
    {
      "name": "EnergySage",
      "version": "0.1.0",
      "devDependencies": {
        //The Task runner itself
        "grunt": "~0.4.1",

        //JS Stuff
        "grunt-contrib-uglify": "~0.2.2",
        "grunt-contrib-concat": "~0.3.0",
        "grunt-contrib-jshint": "~0.7.2",

        //Image Stuff
        "grunt-contrib-imagemin": "~0.4.0",

        //Live Reload Stuff
        "grunt-contrib-watch": "~0.5.3",
        "grunt-contrib-connect": "~0.5.0",
       
        //SASS Stuff
        "grunt-contrib-cssmin": "~0.7.0",
        "grunt-contrib-compass": "~0.7.0"
      },
      "dependencies": {
        "load-grunt-tasks": "~0.2.0"
      }
    }

.. code-block:: javascript

    //Gruntfile.js
    module.exports = function(grunt) {
      grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        compass: {
          dist: {
            options: {
              config: 'config.rb'
            }
          }
        },
        cssmin: {
          combine: {
            files: {
              'css/build/minified/global.css': ['css/*.css']
            }
          }
        },
        jshint: {
          beforeconcat: ['js/*.js', 'lib/**/*.js']
        },
        concat: {
          dist: {
            src: [
              'js/*.js'
            ],
            dest: 'js/build/production.js'
          }
        },
        uglify: {
          build: {
            src: 'js/build/production.js',
            dest: 'js/build/production.min.js'
          }
        },
        imagemin: {
          dynamic: {
            files: [{
              expand: true,
              cwd: 'img/',
              src: ['img/**/*.{png,jpg,gif}'],
              dest: 'img/'
            }]
          }
        },
        watch: {
          options: {
            livereload: true,
          },
          scripts: {
            files: ['js/*.js'],
            tasks: ['concat', 'uglify', 'jshint'],
            options: {
              spawn: false,
            }
          },
          css: {
            files: ['sass/*.scss'],
            tasks: ['compass', 'autoprefixer', 'cssmin'],
            options: {
              spawn: false,
            }
          },
          images: {
            files: ['img/**/*.{png,jpg,gif}', 'img/*.{png,jpg,gif}'],
            tasks: ['imagemin'],
            options: {
              spawn: false,
            }
          }
        },
        connect: {
          server: {
            options: {
              port: 7000,
              base: './'
            }
          }
        },
      });
      require('load-grunt-tasks')(grunt);
      grunt.registerTask('default', ['concat', 'uglify', 'compass', 'cssmin', 'imagemin']);
      grunt.reigsterTask('dev', ['connect', 'watch']);
    };

Django Forms
============

Don't Exclude in Meta
---------------------

Explicitly show fields in Meta (rather than listing fields to exclude).

.. code-block:: python

    class Meta:
        model = MarketQuoteSolarPv1
        fields = (
            'asdf',
            'asdf2',
        )

Required Fields
---------------

Django tries to figure out if a field is required based on the model. If the
model field has blank=True, then the form field will not be required. You can
change this in the form by overriding the init function

.. code-block:: python

    def __init__(self, *args, **kwargs):
        super(PLNForm, self).__init__(*args, **kwargs)
        self.fields['asdf'].required = True
        

Field Labels
------------

The from field label is copied from the `verbose_name` attribute in the model
field.

If you don't want to change a form field label globally then do the following in the template

.. code-block:: html

    {{ form.system_size_wdc|attr:"label:Size of the thing" }}


Change default widget
---------------------

.. code-block:: python

    class MetricsForm(forms.ModelForm):

        class Meta:
            widgets = {
                'system_size_wdc': forms.Choice,
            }


Adding classes to form fields
-----------------------------

Form field classes should be added in the template using the add_class filter,
to achieve a better degree of separation between the markup and forms (whose
primary responsibility is validation not presentation).

.. code-block:: html

    {{ form.system_size_wdc|add_class:"css_class_1 css_class_2" }}

Alternatively attributes can be set in forms.py

.. code-block:: python

    class MetricsForm(forms.ModelForm):

        Meta:
            widgets = {
                'system_size_wdc': HumanIntegerField(attrs={'class': 'table-cell-value'})
            }


It should also be noted that widget_tweaks has filters `field_type` and `widget_type`
that could be used to dynamically creating classes or id's based on the type of
field or type of widget is being displayed.

.. code-block:: python

    <div class="{{ form.system_size_wdc|widget_type }}-container">
        {{ form.system_size_wdc }}
    </div>

Adding other attributes to form fields
--------------------------------------

The purpose of django forms is validation, and although attributes can be added
for forms via a widgets attrs key this logic should really live in the template.

The best way to add validation logic to a form is in the clean method.

.. code-block:: python

    class CPForm(forms.ModelForm):
        def clean_cp_agreement_duration(self):
            cp_agreement_duration = self.cleaned_data['cp_agreement_duration']
            if cp_agreement_duration < 1 or cp_agreement_duration > 100:
                raise ValidationError('Duration out of range!')
            return cp_agreement_duration

A slightly less desirable way, but sometimes neccisary way is to add min/max
values to model fields by overriding the form function like as follows.

.. code-block:: python

    class CPForm(forms.ModelForm):
        cp_agreement_duration = forms.IntegerField(
            label="Duration of Agreement",
            min_value=1, max_value=100,
            widget=TextInput(attrs={'class': 'table-cell-value'}),
        )

The last way to add validation to a field is via __init__ by
adding Min/MaxValueValidator to the field. This has
the slight benefit of not redeclaring the form field type. It has the drawback
of validation logic being put into the wrong area of code. Also if you omit say
'required' from the dictionary you will receive a KeyError, so it's important
to have the correct keys defined in your error_message dictionary. This
method is also undocumented.

.. code-block:: python

    class CPForm(forms.ModelForm):
        def __init__(self, *args, **kwargs):
            super(CPForm, self).__init__(*args, **kwargs)
            self.fields['cp_cents_per_kwh'].required = True
            self.fields['cp_cents_per_kwh'].validators.append(MinValueValidator(1))
            self.fields['cp_cents_per_kwh'].error_messages = {
                'required': 'This Field is Required',
                'min_value': 'Please enter the cost per kWh in cents'
            }

Remove Default value from ModelChoiceField
------------------------------------------

Lets say you have the following bacon types ['maple', 'smoked', uncured'] when
the ModelChoiceField will use a Select widget. The problem is that the HTML
returned is

.. code-block:: html

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

Remove Default value from ChoiceField
-------------------------------------

If the field bacon_types is not required in the model i.e. blank=True then the
blank option is appended.

.. code-block:: python

    class BaconTypesForm(forms.ModelForm):
        class Meta:
            model = BaconTypes
            fields = ('bacon_types',)

        def __init__(self, *args, **kwargs):
            super(BaconTypesForm, self).__init__(*args, **kwargs)
            self.fields['bacon_type'].choices = BACON_TYPE_CHOCIES

