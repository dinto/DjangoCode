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
