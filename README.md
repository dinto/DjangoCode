1)python -m venv virenv
2)virenv\Scripts\activate
3)pip install django
4)django-admin startproject SuperNovaTradingapp . 
5)python manage.py startapp SuperNovaTrading
6)
in settings: 
'SuperNovaTrading', in installed apps 
ALLOWED_HOSTS = ['*']
7)python manage.py runserver
