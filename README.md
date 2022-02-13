
//this repo is under construction

OS: Ubuntu 20.04 LTS
IDE : Pycharm

//set up django in ubuntu

0. 
>> sudo apt install python3.8


1. 
>> sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 1

2. 
>> sudo apt install python3-pip -y

3. 
>> sudo update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1


4. 
>> pip install django==3.0.0

5. to check
>> django-admin --version

ref link : https://www.howtoforge.com/tutorial/how-to-install-django-on-ubuntu/



//set up in Pycharm

1. open pycharm
2. create a django project
3. select location. (path should not have any space seperated named folder)
4. select create/install in default settings
5. then in terminal

to test django
i.	
>> python manage.py runserver

Starting development server at http://127.0.0.1:8000/

click the link

in terminal 

    ctrl + C to exit
 
ii. 	
>> python manage.py migrate

iii.	
> python manage.py runserver


//start guideline

1. 
in django we can create module. its called app.
So, first we will create a app
to make e newapp(let's say it's name is homepage)


>> python manage.py startapp homepage

in settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'homepage', #add this here
]

2.
in newapp homepage create e python file 'urls.py'

*copy everything from urls.py of mainapp to newly created 'urls.py' of homepage app.
(delete the admin part of path)

3.
in mainapp
	in url.py


from django.urls import path, include

urlpatterns = [
    path('', include('homepage.urls')),
    path('admin/', admin.site.urls),
]

4.
in homepage(new app)
	in urls.py


from django.contrib import admin
from django.urls import path,include
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]


5.
in new app(homepage)
	in views.py


from django.shortcuts import render


def home(request):
    return render(request,'index.html')

6. 
create index.html page in the templates folder


***** to add templates follow "STEPS TO ADD TEMPLATE" txt file**************


1. copy all html files to template folder of the project

2. copy all js,css,plugin,images folder to static folder of the project

3. in settings.py ADD

STATIC_URL = 'static/'

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static')
]

STATIC_ROOT = os.path.join(BASE_DIR, 'assets')

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media') 

4. in urls.py of mainapp add

from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('homepage.urls')),
    path('admin/', admin.site.urls),
]

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

5. python manage.py collectstatic (in the terminal(here i am using pycharm terminal))

6. change to html pages to add static

in the firstline of html code

    add {% load static %} 


then in js and css and images files

update link, script, image tags like this===>


<link rel="stylesheet" href="assets/css/main.css" /> 
update this to: 
==> <link rel="stylesheet" href="{%static 'assets/css/main.css' %}" />

<script src="assets/js/jquery.min.js"></script> 
==> <script src="{% static 'assets/js/jquery.min.js' %}"></script>

<span class="image main"><img src="images/pic03.jpg" alt="" /></span> 
==> <span class="image main"><img src="{% static 'images/pic03.jpg' %}" alt="" /></span>


//database add

1. create class in models.py of your app
like this: 
class Myinfo(models.Model):
    name = models.CharField(max_length=100)
    address = models.CharField(max_length=200)

    def __str__(self):
        return self.name 


1.1.
in admin.py of your app
					i. admin.site.register(class_name)

to register Myinfo:
                    admin.site.register(Myinfo)

2. to see the class/ changes in class ==>

                   >> python manage.py makemigrations  

3. to add classes to database==>
                   >> python manage.py migrate 

4. for images add in the settings
		i. MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
		ii. MEDIA_URL = '/media/'

5. in urls.py add
		i. urlpatterns += static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT)

6. To collect statics :
	i.  
    >> manage.py collectstatic
	

to create a admin:

		>> python manage.py createsuperuser

then enter username, email, password

7.
>> python manage.py runserver

go to:
	
	http://127.0.0.1:8000/admin

enter username and password to log in

8. under your app, you can see your Table name

9. click to enter and in top right you can +add button. from here, you can add new data.
