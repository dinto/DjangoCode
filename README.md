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
