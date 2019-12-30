# Pivotal Cloud Foundry Configuration

### Step 1 - Create runtime.txt with following code.
-   This File will define the version of Python you want to install in Container.
~~~
python-3.5.x
~~~

### Step 2 - Create requirements.txt with following code.
-   This will define all the dependencies you want for your app.
~~~
dj-database-url
dj-static
Django
django-toolbelt
djangorestframework
gunicorn
whitenoise
mysqlclient
psycopg2
static3
~~~

### Step 3 - Create Manifest.yml file with following code.
-   This file will be blueprint of your app deployment.
~~~
---
applications:
- name: MyDjangoPythonApp
~~~

### Step 4 - Add Below lines in your myproject/settings.py file.
~~~
PROJECT_ROOT = os.path.dirname(os.path.abspath(__file__))
STATIC_ROOT = os.path.join(PROJECT_ROOT, 'staticfiles')
~~~

and add below in MIDDLEWARE section
~~~
'whitenoise.middleware.WhiteNoiseMiddleware',
~~~

### Step 5 - Change below line accordingly in myproject/settings.py file.
~~~
ALLOWED_HOSTS = [ '*' ]
~~~

### Step 6 - Modify the content of manage.py according to below lines.
~~~
#!/usr/bin/env python
"""Django's command-line utility for administrative tasks."""
import os
import sys


if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "myproject.settings")

    from django.core.management import execute_from_command_line

    execute_from_command_line(sys.argv)
   
~~~

### Step 7 - Add Procfile
~~~
web: python manage.py migrate && python manage.py collectstatic --noinput && gunicorn myproject.wsgi --log-file -
~~~ 

### Step 8 - Push App
~~~
cf push
~~~

### Step 9 - Troubleshoot
-   If in any case you need to create super user in PCF environment.
-   Need to do ssh into app container using below command.
~~~
cf ssh MyDjangoPythonApp -t -c "/tmp/lifecycle/launcher /home/vcap/app bash `'"
~~~ 

-   Then run createsuperuser command there.
~~~
python manage.py createsuperuser
~~~
