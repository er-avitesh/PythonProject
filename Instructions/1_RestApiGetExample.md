# Lab 1 :
##Python Django Based REST API with GET Operation

### Step 1 - Create a Project
-   Create any directory where you would like to have your projects.
-   Open any terminal and go to your working directory.
-   Execute below command to create Django project.
~~~python
django-admin startproject myProject
~~~
<b>NOTE:</b>  myProject is teh name of project i have given.
-   This will create a project directory consist of a sub directory named myProject and manage.py
-   Your Sub Directory myProject will include files like __init__.py , settings.py , url.py , wsgi.py .
### Step 2 - Configure IDE
-   I will be using IntelliJ PyCharm IDE for my Projects.
-   Open the Project in PyCharm.
-   Open Terminal inside Pycharm from VIEW -> Tool Windows -> Terminal
-   Install django rest framework here too using below command.
~~~python
pip install djangorestframework
~~~

### Step 3 - Create sub Web Project
-   Go to your Pycharm Terminal.
-   Execute below command to create a webapp project.
~~~python
python manage.py startapp webapp
~~~
-   This will create webapp sub directory inside your project directory parallel to myProject sub directory.

### Step 4 - Configure webapp in your project settings.
-   Open settings.py from myprojectsub directory.
-   Go to section INSTALLED_APPS and add following dependency.
~~~
'rest_framework',
'webapp'
~~~

### Step 5 - Create Model class

-   Go to models.py in webapp. 
-   Add below code inside that.
~~~python
from django.db import models

class employees(models.Model):
    firstname=models.CharField(max_length=10)
    lastname=models.CharField(max_length=10)
    emp_id=models.IntegerField()

    def __str__(self):
        return self.firstname
~~~

### Step 6 -  Setup Admin panel of Django for CRUD Operations. 
-   Open admin.py from webapp.
-   Add below code in admin.py
~~~python
from django.contrib import admin
from .models import employees

admin.site.register(employees)
~~~

### Step 7 - Register your Schema
-   Go to Terminal.
-   Execute below commands.
~~~python
python manage.py makemigrations
python manage.py migrate
~~~
-   Above command will create and update schema with DB.

 
### Step 8 - Setup Super user to perform CRUD operations.
-   Go to Terminal 
-   Execute below command
~~~python
python manage.py createsuperuser
~~~
-   Add username , email and password when prompted.

### Step 9 - Check your work.
-   Execute below command to run the server.
~~~python
python manage.py runserver
~~~
-   Go to Any browser, i recommend Chrome.
-   Hit the URL displayed at your console when ran the server. For me it was http://127.0.0.1:8000
-   You should see Django Welcome Page.
-   As we have configured the admin panel, try http://127.0.0.1:8000/admin.
-   Enter your superuser userid and password.
-   You should see Django Admin Page.
-   You should be able to see your model there and if you click in there. You will see inside.
-   Try adding the data in there and you can perform CRUD operations from there too.

### Step 10 - Create Response Object.
-   Go to webapp directory.
-   Create serializers.py with below code in that.
~~~python
from rest_framework import serializers
from .models import employees


class employeesSerializer(serializers.ModelSerializer):

    class Meta:
        model = employees
# Below line will make your response contain only firtsname and lastname 
        fields = ('firstname', 'lastname')
# # Below line will make your response contain everything it get back from model.
        #fields = '__all__'  
~~~

### Step 11 - Create View aka controller class
-   Go to views.py in webapp.
-   Enter below code.
~~~python
from django.shortcuts import render
from django.http import HttpResponse
from django.shortcuts import get_object_or_404
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import employees
from .serializers import employeesSerializer


class employeeList(APIView):

    def get(self, request):
        employees1 = employees.objects.all()
        serializer = employeesSerializer(employees1, many=True)
        return Response(serializer.data)

    def post(self):
        pass
~~~

### Step 12 - Configure Endpoint.
-   Open urls.py from myproject.
-   Enter import and code like below.
~~~python
from django.contrib import admin
from django.urls import path
from rest_framework .urlpatterns import format_suffix_patterns
from webapp import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('employees/', views.employeeList.as_view()),
]
~~~

### Step 13 - Check the Work
-   Go to browser.
-   Hit the server url and port with /employees endpoint now like http://127.0.0.1:8000/employees/
-   You should see the response back with data you added recently from Django admin panel.

NOTE : By Default Python Dango is using in memory DB sqlite.
