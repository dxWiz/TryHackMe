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
Do a nmap scan and you will notice that the http server is running on port 8000. Go to $IP:8000 but you will get error as following:

        DisallowedHost at /

        Invalid HTTP_HOST header: '10.10.156.98:8000'. You may need to add '10.10.156.98' to ALLOWED_HOSTS.

        Request Method: 	GET
        Request URL: 	http://10.10.156.98:8000/
        Django Version: 	2.2.12
        Exception Type: 	DisallowedHost
        Exception Value: 	

        Invalid HTTP_HOST header: '10.10.156.98:8000'. You may need to add '10.10.156.98' to ALLOWED_HOSTS.

        Exception Location: 	/usr/local/lib/python3.6/dist-packages/django/http/request.py in get_host, line 111
        Python Executable: 	/usr/bin/python3
        Python Version: 	3.6.9
        Python Path: 	

        ['/home/django-admin/messagebox',
         '/usr/lib/python36.zip',
         '/usr/lib/python3.6',
         '/usr/lib/python3.6/lib-dynload',
         '/usr/local/lib/python3.6/dist-packages',
         '/usr/lib/python3/dist-packages']
         
We need to fix this error. Let's SSH with the given username `django-admin` and password `roottoor1212`. Once you are in, go to messagebox settings and edit settings.py
 
        django-admin@py:~$ cd messagebox
        django-admin@py:~/messagebox$ ls
        db.sqlite3  lmessages  manage.py  messagebox
        django-admin@py:~/messagebox$ cd messagebox
        django-admin@py:~/messagebox/messagebox$ ls
        home.html  __init__.py  __pycache__  settings.py  urls.py  views.py  wsgi.py
        django-admin@py:~/messagebox/messagebox$ nano settings.py
        
Add the machine IP to `ALLOWED_HOSTS = ['$IP', '0.0.0.0', '127.0.0.1']` and refresh the webpage and you will see something as below

<p align="center">Message box v1.1<br><br>Hi! Welcome back to your inbox. Seems like you got a new message!<br><br>Check it out here:<br><br>Messages</p>

Go back to the terminal and set a password for your app with the command `python3 manage.py createsuperuser` and login via `http://$IP:8000/admin` page with the newly set superuser password. Click `Users` and get the first Flag.

The second Flag is in `StrangeFox` folder, just go to that folder and get it. 

For the third and hidden Flag the hint given is `Did you see any identical files?` so what we can do is look for any file with the same name. To get the flag, just use `grep -r THM` and you should be able to find the flag.

Congratulations.

