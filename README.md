# python-social-auth with Django 1.9

## Quickstart

```bash
pip install -r requirements.txt
django-admin.py startproject django_social
cd django_social
./manage.py startapp django_social_app
pip install python-social-auth
mkdir templates
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

## Changes made to settings.py

Add template dir, and context_processors.

```python
TEMPLATES = [ 
    {   
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'social.apps.django_app.context_processors.backends',
                'social.apps.django_app.context_processors.login_redirect',
            ],
        },
    },  
]
```

Add to installed apps:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django_social',
    'social.apps.django_app.default',
]
```

Add authentication backends:

```python
AUTHENTICATION_BACKENDS = (
    'social.backends.facebook.FacebookOAuth2',
    'social.backends.twitter.TwitterOAuth',
    'django.contrib.auth.backends.ModelBackend',
)
```


Add to URL patterns:

```python
from django.conf.urls import url, include
from django.contrib import admin
from django_social_app.views import login, home, logout

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url('', include('social.apps.django_app.urls', namespace='social')),
    url(r'^$', login),
    url(r'^home/$', home),
    url(r'^logout/$', logout),
]
```

Get Authentication keys:
* [Facebook](http://python-social-auth.readthedocs.io/en/latest/backends/facebook.html)
* [Twitter](http://python-social-auth.readthedocs.io/en/latest/backends/twitter.html)
* [Others](http://python-social-auth.readthedocs.io/en/latest/backends/index.html)
