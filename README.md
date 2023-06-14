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
----------------------
    for mysql
----------------------
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.mysql',
		'NAME': 'KTCC_DATABASE',
		'USER': 'KTCC',
		'PASSWORD': 'ktcc@2023',
		'HOST':'localhost',
		'PORT':'3306',
	}
}
----------------------
    End Mysql
----------------------
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
models.py:
    from django.db import models

    # Create your models here.

    class Recommendation(models.Model):
      tradingSymbol = models.CharField(max_length=250)
      signal = models.CharField(max_length=250)
      time  = models.CharField(max_length=250)
      price  = models.CharField(max_length=250)
      Stoploss = models.CharField(max_length=250)
      Target = models.CharField(max_length=250)
      count = models.CharField(max_length=10)
      is_Target_hit = models.BooleanField()
      is_Stoploss_hit = models.BooleanField()
      #each record name showing tradingsymbol and time base 
      def __str__(self):
        return  self.tradingSymbol + '' + self.time


python manage.py makemigrations
python manage.py migrate



to show admin area:
    
    
    
admin.py:
from .models import Recommendation
admin.site.register(Recommendation)

Pull Data From The Database :
inside view.py:
from .models import Recommendation
all_recomendations = Recomendation.objects.all
return render(request,index.html,{'all':all_recomendations}

index.html
{% for item in all %}
{{item.tradingsymbol}}
{% endfor %}

    
    
**save to database**
from .models import Student
if request.method=="POST":
    name =request.POST['name']
    student=Student(name=name)
    student.save()
or 
    from SuperNovaTrading.models import Recommendation
                      recommendation= Recommendation(
                        tradingSymbol=tradingSymbol,signal='buy',time=timecal,price=Technical.round_nearest(src[i+1]),
                        Recommendation_date= datetime.now(),Target1=Technical.round_nearest(src[i+1]+(src[i+1]*0.003)),
                        SL1=Technical.round_nearest(src[i+1]-(src[i+1]*0.01)),Recommended_by='dinto',count=i,
                        is_StockIntraday=True
                        )
                      recommendation.save()
more study:
https://docs.djangoproject.com/en/4.2/topics/db/models/
https://docs.djangoproject.com/en/4.2/ref/models/fields/
===============================================================================
Authentication page
===============================================================================
Login page: 
1)login.html
    <form method="post">
        <h3>Login Here</h3>
        {% csrf_token %} 
        <label for="username">Username</label>
        <input type="text" placeholder="Enter Username" id="username" name="username">
        <label for="password">Password</label>
        <input type="password" placeholder="Password" id="password" name="pass">
        <button type="submit">Log In</button>
        <a href="{% url 'signup' %}" >Create a account</a>
    </form>

2)url:
   path('login/',views.LoginPage,name='login'),

3)view
from django.shortcuts import render,HttpResponse,redirect
from django.contrib.auth.models import User
from django.contrib.auth import authenticate,login,logout
from django.contrib.auth.decorators import login_required

def LoginPage(request):
    if request.method=='POST':
        username=request.POST.get('username')
        pass1=request.POST.get('pass')
        user=authenticate(request,username=username,password=pass1)
        if user is not None:
            login(request,user)
            return redirect('home')
        else:
           return redirect('login')
           # return HttpResponse ("Username or Password is incorrect!!!")

    return render (request,'Authentication/login.html')

4) in view,above home function 
@login_required(login_url='login')

signup page:
1)signup.html
 <form action="" method="post">
        {% csrf_token %} 
        <h3>Signup Here</h3>

        <label for="username">Username</label>
        <input type="text" placeholder="Username" name="username" id="username">

        <label for="email">Email</label>
        <input type="email" placeholder="Email or Phone" name="email" id="email">

        <label for="password1">Password</label>
        <input type="password" placeholder="Password" id="password1" name="password1">


        <label for="password2">Confrom Password</label>
        <input type="password" placeholder="Confrom Password" id="password2" name="password2">
        <button type="submit">Signup</button>
        
        <a href="{% url 'login' %}" >i have already account</a>
    </form>
2)url:
    path('signup/',views.SignupPage,name='signup'),
3)view:
def SignupPage(request):
    if request.method=='POST':
        uname=request.POST.get('username')
        email=request.POST.get('email')
        pass1=request.POST.get('password1')
        pass2=request.POST.get('password2')

        if pass1!=pass2:
            return HttpResponse("Your password and confrom password are not Same!!")
        else:

            my_user=User.objects.create_user(uname,email,pass1)
            my_user.save()
            return redirect('login')
    return render (request,'Authentication/signup.html')

url:
    path('home/',views.SuperNovaTrading,name='home'),
    
logout:
1)url:
    path('logout/',views.LogoutPage,name='logout'),
    
2)view:
def LogoutPage(request):
    logout(request)
    return redirect('login')
3)in home page:
<a href="{% url 'logout' %}" class="btn btn-primary">Logout</a>




#=================================
  admin page design
 ================================
    
    url.py
   
    
admin.site.site_header = 'SuperNova Administration dashboard'                   
admin.site.index_title = 'SuperNova'                 
admin.site.site_title = 'SuperNova Administration'

    
    
   
   
#===============================
    Pagination
#===============================
inside view:
 from django.core.paginator import Paginator
 
Videos_models= VideoLink.objects.all()
p = Paginator(Videos_models,1)
page = request.GET.get('page')
Videos = p.get_page(page)
nums = "a" * Videos.paginator.num_pages
return render(request,'welcome.html',{'Videos':Videos,'nums':nums})
    
    
  Html pge:
      <p style="font-family: Merriweather;font-size: 22px;">Matches Videos</p>
  <div class="row">
    {% for Match_Videos in Videos %}
    <div class="card" style="width: 360px;">
      <iframe class="embed-responsive-item"  width="360" height="315" src="{{Match_Videos.Youtube_Link}}" title="YouTube video player" 
          frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
      </iframe>
      <div class="card-body">
        <h5 class="card-title"  style="color:#2A6FAB; font-family: Merriweather;font-size: 20px;">{{Match_Videos.Title_Name}}</h5>
      </div>
    </div>
    &nbsp;
    {% endfor %}   
  </div>
  <nav aria-label="Page navigation example">
    <ul class="pagination">
      {% if Videos.has_previous %}
      <li class="page-item"><a href="?page=1"  class="page-link">&laquo First</a></li>
      <li class="page-item"><a href="?page={{Videos.previous_page_number}}"  class="page-link"> Previous</a></li>
      {% endif %}      
      {% for i in nums %}
       <li class="page-item"><a class="page-link" href="?page={{forloop.counter }}">{{forloop.counter}}</a></li>
      {% endfor%}
      {% if Videos.has_next %}
      <li class="page-item"><a href="?page={{ Videos.next_page_number }}"  class="page-link"> Next</a></li>
      <li class="page-item"><a href="?page={{ Videos.paginator.num_pages }}"  class="page-link">Last &raquo </a></li>
      {% endif %}
    </ul>
  </nav>
    
    
 #===============================
   End Pagination
#===============================
    
    
#===================================
    Tab design
#===================================
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ/JpcUGGOn+Y7RsweNrtN/tE3MoK7ZeZDyx" crossorigin="anonymous"></script>
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<br>
<br>

    <br>
    <br>

    <!-- Tabs navs aria-current="page" -->
<ul class="nav nav-tabs nav-justified mb-3" id="ex1" role="tablist">
  <li class="nav-item" role="presentation">
    <a
      class="nav-link active"
      id="ex3-tab-1"
      data-bs-toggle="tab"
      href="#ex3-tabs-1"
      role="tab"
      aria-controls="ex3-tabs-1"
      aria-selected="true"
      >Sold Players</a
    >
  </li>
  <li class="nav-item" role="presentation">
    <a
      class="nav-link "
      id="ex3-tab-2"
      data-bs-toggle="tab"
      href="#ex3-tabs-2"
      role="tab"
      aria-controls="ex3-tabs-2"
      aria-selected="false"
      >UnSold Players</a
    >
  </li>
</ul>
<!-- Tabs navs -->

<!-- Tabs content -->
<div class="tab-content" id="ex2-content">
  <div  class="tab-pane fade show active"  id="ex3-tabs-1"  role="tabpanel"  aria-labelledby="ex3-tab-1">
    <form class="form-inline my-2 my-lg-0"  action="{% url 'BidStatus' %}" method="GET">
      <div class="row">
        <div class="col-8">
          <input class="form-control mr-sm-2" type="text" name="query" placeholder="Search" aria-label="Search">
  
        </div>
        <div class="col-4">
          <button class="btn btn-outline-success my-2 my-sm-0" name="search" type="submit">Search</button>
        </div>
        
  
      </div>
     
     
    </form>
  
    {% for Sold_Player in Sold_Players %}
    <div class="card" style="width: 24rem; margin: 10px"  id="image_container">
      <div class="row">
          <div class="col-6">
              <img src="/static/{{Sold_Player.Player_name.Profile_Pic}}" class="player" >
              
          </div>
          <div class="col-6">   
            <h1 class="card-title">{{ Sold_Player.Player_name.name }}</h1>
            <p class="card-text"> {{ Sold_Player.Player_name.Batting_style }}</p>
            <p class="card-text"> {{ Sold_Player.Player_name.Bowling_style }}</p>
          </div>
      </div>
      <div class="card-body" style="padding: 0px; background-color: #9999">   
        <div class="row">
            <div class="col-6">
                {{ Sold_Player.Team_Name}}
                <p class="card-text"> Bid point:{{ Sold_Player.Sold_Point }}</p>

            </div>
            <div class="col-6">
                <img src="/static/{{Sold_Player.Team_Name.Team_Logo}}" class="team">
            </div>
        </div>
         
      </div>
    </div>
    {% endfor %}
  </div>

<!-- Tab unsold players-->

  <div  class="tab-pane fade" id="ex3-tabs-2" role="tabpanel" aria-labelledby="ex3-tab-2">

    <form class="form-inline my-2 my-lg-0"  action="{% url 'BidStatus' %}" method="GET">
      <div class="row">
        <div class="col-8">
          <input class="form-control mr-sm-2" type="text" name="query_unsold" placeholder="Search" aria-label="Search">
        </div>
        <div class="col-4">
          <button class="btn btn-outline-success my-2 my-sm-0" name="search_unsold" type="submit">Search</button>
        </div>
      </div>
    </form>
  
    {% for Unsold_Player in Unsold_players %}
    <div class="card" style="width: 24rem; margin: 10px"  id="image_container">
      <div class="row">
          <div class="col-6">
              <img src="/static/{{Unsold_Player.Player_name.Profile_Pic}}" class="player" >
              
          </div>
          <div class="col-6">   
            <h1 class="card-title">{{ Unsold_Player.Player_name.name }}</h1>
            <p class="card-text"> {{ Unsold_Player.Player_name.Role }}</p>
            <p class="card-text"> {{ Unsold_Player.Player_name.Batting_style }}</p>
            <p class="card-text"> {{ Unsold_Player.Player_name.Bowling_style }}</p>
          </div>
      </div>
    </div>
    {% endfor %}

  </div>
</div>
    
#===============================================================
    End Tab Design
#===============================================================
#===============================================================
    column wise iteration design
#===============================================================
    {% for chunk in posts %}
    <div class="row">
        {% for post in chunk %}
            <div class="col-sm">
                <div class="row" style="background-color: #F2F2F2;padding: 3px;border-radius: 25px; width:90%">
                    <div class="col-sm-4" >
                        <img src="/static/{{post.Team_Logo}}" style=" width:100%; height:100%; border-radius: 10px;" alt="Team Logo">
                    </div>
                    <div class="col-sm-8" style="font-family: Merriweather; font-size: 20px;" >
                        <p >{{post.Team_Name}} </p>
                        <p class="text-justify" id="text">
                            {{post.Short_Name}}
                        </p>
                        {% for Remaining_Points in Remaining_Point %}           
                            {%if Remaining_Points.Team_Name.id == post.id %}
                                <span style="color: red;">Remaining Point: {{Remaining_Points.Available_Point}}</span>
                            {% endif %}
                        {% endfor %}
                        <div class="row">
                            <a href="{% url 'Team_players' post.id %}">Players</a>
                        </div>
                       
            
                    </div>
                    
                </div>
                &nbsp; &nbsp;
            </div>
        {% endfor %}
    </div>
{% endfor %}


in view.py
    posts = list(TeamInfo.objects.all())
    posts = [posts[i:i+2] for i in range(0, len(posts), 2)]
#===============================================================
   End column wise iteration design
#===============================================================
    
    
    
============================
CSV or EXCEL file download
=========================== 
in view.py

import csv


def players_csv(request):
response =HttpResponse(content_type='text/csv')
response['Content-Disposition'] ='attcahment;filename=playersregistration.csv'
writer =csv.writer(response)
players=PlayerInfo.Objects.all()
writer.writerow(['Name','Date of Birth','Age','Place','Phone Number','Mail Id','Role','Batting Style','Bowling Style','is home ground player','is icon player'])

for player in players:
  writer.writerow([player.name,player.dob,player.age,player.place,player.phone_number,player.mail_id,player.Role,player.Batting_style,player.Bowling_style,player.is_home_ground_player,player.is_icon_player])

return response



url.py:
    path('players_csv/', views.players_csv, name='players_csv'),

template:
==================================================================================================
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
  ==============
admin css problem solving after hosting 
==============
pip install whitenoise

In setting file:
DEBUG = False

MIDDLEWARE[
'whitenoise.middleware.WhiteNoiseMiddleware',
]

STATICFILES_STORAGE='whitenoise.storage.CompressedManifestStaticFilesStorage'
------------------
python manage.py collectstatic     
this will add 132 files to static folder
