===============================================================================
initial step 
===============================================================================


1)python -m venv virenv   <br />
2)virenv\Scripts\activate  <br />
3)pip install django     <br />
4)django-admin startproject SuperNovaTradingapp .  <br />
5)python manage.py startapp SuperNovaTrading   <br />

6)in settings: 
'SuperNovaTrading', in installed apps 
ALLOWED_HOSTS = ['*']
<br />
7)python manage.py runserver
<br />

8)admin setup:
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser

<br />
output:
Username (leave blank to use 'hp'): admin
Email address: dinto.t.d@gmail.com
Password:
Password (again):
Superuser created successfully.




9)in view.py:

def SuperNovaTrading(request):
    return render(request,'index.html',{})

10)mkdir SuperNovaTrading/templates/

create index.html => SuperNovaTrading/templates/index.html

11)create SuperNovaTrading/urls.py

from django.urls import path
from SuperNovaTrading import views

urlpatterns = [
    path('', views.SuperNovaTrading, name='index'),
]

12)
inside SuperNovaTradingapp/urls.py=>
from django.urls import path, include
path('', include('SuperNovaTrading.urls')),


13)#Instead of having to import Bootstrap styles into every app, we can create a template or set of templates that are shared by all the apps. 

mkdir SuperNovaTradingapp/templates/
create html under=> SuperNovaTradingapp/templates/base.html

inside base.html=>
add following lines inside base.html=>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.1.3/dist/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
<!DOCTYPE html>
<html>
    <head>
        <title>SuperNovaTrading</title>
    </head>

    <body>
{% block content %}

{% endblock content%}
</body>
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.14.3/dist/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.1.3/dist/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>



14)

change SuperNovaTrading->templates->index.html =>

{% extends "base.html" %}

{% block content %}

{% endblock content %}

15)
SuperNovaTradingapp/settings.py=>, update TEMPLATES:
'DIRS': ["SuperNovaTradingapp/templates/"],




16) static file adding:
create folder static and inside css and js ,images
create file js 
in settings:

import os
BASE_DIR = os.path.dirname(os.path.dirname(__file__))
PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))

STATICFILES_DIRS=[os.path.join(BASE_DIR,'static')]
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(PROJECT_ROOT, 'db.sqlite3'),
    }
}

<br />

17)if error showing like this : django.db.utils.OperationalError: no such table: django_session
 python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
    
    
solution: 
in index.html:
{% load static %}
<script src={% static 'js/dashboard.js' %}></script>
<link href={% static 'css/dashboard/dashboard.css' %} rel="stylesheet" type="text/css" />

===============================================================================
model creation
===============================================================================

===============================================================================
login page
===============================================================================
