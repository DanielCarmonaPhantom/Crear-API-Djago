# Tutorial para crear una API con Django


## Crear el proyecto del directorio
```Bash
mkdir tutorial
cd tutorial
```

## Creamos el ambiente virtual
```Bash
python3 -m venv env
source env/bin/activate  # On Windows use `env\Scripts\activate`
```

## Instalamos Django y DjangoRestFramework
```Bash
pip3 install django
pip install djangorestframework
```


## Crearemos el proyecto y una aplicación
```Bash
django-admin startproject tutorial .  # Note the trailing '.' character
cd tutorial
django-admin startapp quickstart
cd ..
```