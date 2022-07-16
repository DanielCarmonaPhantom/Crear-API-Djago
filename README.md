# Tutorial para crear una API con Django

## Configuración del proyecto 

### Crear el proyecto del directorio
```Bash
mkdir tutorial
cd tutorial
```

### Creamos el ambiente virtual
```Bash
python3 -m venv env # On Windows use `py`
source env/bin/activate  # On Windows use `env\Scripts\activate`
```

### Instalamos Django y DjangoRestFramework
```Bash
pip3 install django # On Windows use `pip`
pip3 install djangorestframework
```


### Crear un proyecto y una aplicación
```Bash
django-admin startproject tutorial .  # Note the trailing '.' character
cd tutorial

django-admin startapp quickstart
```

### Crear una migración
```Bash
python3 manage.py migrate
```

### Crear un super Usuario
```Bash
python3 manage.py createsuperuser
```
Para fines del tutorial, utilizaremos `admin` como usuario y de password `password123`

## Serializadores 

Dentro de quickstart vamos a crear un archivo llamado `serializers.py`

```Python
from django.contrib.auth.models import User, Group
from rest_framework import serializers


class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'groups']


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ['url', 'name']
```

## Views
Para nuestras vistas utilizaremos
```Python
from django.contrib.auth.models import User, Group
from rest_framework import viewsets
from rest_framework import permissions
from tutorial.quickstart.serializers import UserSerializer, GroupSerializer


class UserViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows users to be viewed or edited.
    """
    queryset = User.objects.all().order_by('-date_joined')
    serializer_class = UserSerializer
    permission_classes = [permissions.IsAuthenticated]


class GroupViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows groups to be viewed or edited.
    """
    queryset = Group.objects.all()
    serializer_class = GroupSerializer
    permission_classes = [permissions.IsAuthenticated]
```

## Urls

Ahora en nuestras urls del proyecto
```Python
from django.urls import include, path
from rest_framework import routers
from tutorial.quickstart import views

router = routers.DefaultRouter()
router.register(r'users', views.UserViewSet)
router.register(r'groups', views.GroupViewSet)

# Wire up our API using automatic URL routing.
# Additionally, we include login URLs for the browsable API.
urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```

## Paginación
Para que tengamos paginación en nuestra api, agregamos a settings
```Python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```

## Settings
Agregamos a las installed_apps
```Python
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

## Prueba
Corremos el servidor
```Python
python3 manage.py runserver
```
