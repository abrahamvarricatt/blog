Serving Static HTML for Development
###################################

:date: 2015-03-31 13:16
:modified: 2015-03-31 13:16
:tags: python, django
:category: programming
:author: Abraham_V 
:summary: The commands used to serve a folder with Python, and how to do something similar with Django

Assume that we are working a file called "index.html" (shown below) which needs to be rendered on a web-browser. How can this be accomplished?

.. code-block :: html

  <!DOCTYPE html>
  <html>
  <head lang="en">
    <meta charset="UTF-8">
    <title>A title</title>
  </head>
  <body>
    hello ! And here is more stuff.
  </body>
  </html>

The simplest and most straight-forward way, is to just open the file using the browser's "Open file" option. This works for simple stuff, but what-if we had a more complicated site that we wanted to actually server using a server on our development machine? We can use Python's in-build web server! Just navigate to the folder where our HTML is located and run,

.. code-block :: bash

  (python2.7) python -m SimpleHTTPServer
  (python3) python3 -m http.server

Our web-pages can now be accessed from http://127.0.0.1:8000 .

Another (admittedly convoluted) option is to use the debugging mode in Django's development server. To demonstrate, lets start up a new django project;

.. code-block :: bash

  $ django-admin.py startproject mysite
  $ cd mysite/
  $ vim mysite/settings.py  

Make sure to edit the settings to reflect these values,

.. code-block :: python

  import os
  BASE_DIR = os.path.dirname(os.path.dirname(__file__))
  STATIC_URL = '/static/'
  STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
  )

There's another file we need to edit too,

.. code-block :: bash

  $ vim mysite/urls.py

Here's how it should look like,

.. code-block :: python

  from django.conf import settings
  from django.conf.urls.static import static  
  urlpatterns = patterns('') + static(settings.STATIC_URL,
                                    document_root=settings.STATIC_ROOT)

Make the static folder and add stuff to it,

.. code-block :: bash

  $ mkdir static
  $ vim static/index.html

At this point if you run the development server,

.. code-block :: bash

  $ python manage.py runserver

And point your browser to http://127.0.0.1:8000/static/index.html you'll be able to see your static files. 

It should be obvious, but this is a VERY bad way of hosting static files in a production environment. So why bother? For the fun of it! :)



