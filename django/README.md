***

Author: dxWiz  
Date: 05 Apr 2022

***



# Task 1 Unit 1: Introduction  
Read the introduction  

# Task 2 Unit 2 - Getting started
Create an app with the command `python3 manage.py startapp forms`

To allow project to run on local network, use this command `python3 manage.py runserver 0.0.0.0:8000`

# Task 3 Unit 3 - Creating a website

I started a django project with this command `django-admin startproject testing` and migrate the changes with the command `python3 manage.py migrate` then head over to the project setting folder(the folder having the same name as your project and within the project folder) and make changes to settings.py and urls.py as following:

    #settings.py
    INSTALLED_APPS = [
      'forms',
      #....
    ]

<!-- -->

    #urls.py
    from django.contrib import admin
    from django.urls import include, path
    
    urlpatterns = [
      path('forms/', include('forms.urls')),
      path('admin/', admin.site.urls),
    ]

Please do not migrate any changes yet and ensure that your app folder(my case I have a I named my app `forms`) has both `urls.py` and `views.py`

    #urls.py
    from django.urls import path
    
    from . import views
    
    app_name = 'forms'
    urlpatterns = [
      path('', views.index, name='index'),
    ]
    
<!-- -->    

    #views.py
    from django.http import HttpResponse
    
    def index(request):
      return HttpResponse("hello, World!")

Migrate the changes now, you shouldn't see any error if everything is done correctly

# Task 4 Unit 4 - Concluding  
Check the github page for the Flag.

# Task 5 Unit 5 - CTF 
