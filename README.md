DRF_django_rest  /    DJANGO REST-API   🤖🤖🤖🤖

Crear un entorno virtual para aislar lo que estoy instalando. 

        #1. pip install virtualenv

Crear venv.
        
        #2. python -m virtualenv venv

Creacion de projecto.

        #3. django-admin startproject (NOMBRE)

        NOTA: Al final del comando colocar un 
        ( . ) para que la cree fuera del entorno.

Creacion de la Aplicación.

        NOTA: Vamos a crear un APi que permita tener proyectos, 
        crear proyectos, eliminarlos y actualizarlos.

        #4. python manage.py startapp (NOMBRE)


Ahora en el projecto nos vamos a "settings.py" a  agregar la 
aplicacion  ("NOMBRE") Y ("rest_framework") 

       INSTALL-APP

       'NOMBRE',
       'rest_framework'

MODELO-DE-DATOS ==>  models.py 

Se crea este projecto para que se pueda generar la tabla y 
luego poder crear la APi...

       class project(models.Model):
           title = models.CharField(max_length=200)
           description = models.TextField()
           technology = models.CharField(max_length=200)
           created_at = models.DataTimeField(auto_now_add=True)

Despues de haber hecho esto "Migramos" 
             
           python manage.py makemigrations
           python manage.py migrate

!!CREACION DE SERIALIZERS Y VIEW-SET!! 

/SECCION API/

Los view-set = Son formas en las que Django va a poder 
convertir los datos de python en objetos "Json" que 
luego va a poder ser consultados por el cliente.

Tambien nos permite seleccionar o nos permite decir quien 
va a poder acceder a estos datos. 🤯🤯🤯


CREACION DE "SERIALIZERS.PY" EN LA APP:

Serializers.py = Nos permiten llamar un modelo especial de rest-framework.

       #1.   from rest_framework import serializers
       #2.  from .models import project

       #3.   class ProjectSerializer(serializers.ModelSerializer):
       #4.     class Meta:
       #5.        model = project
       #5.        fields = ('id', 'title', 'description','technology', 'created_at')
       #6.        read_only_fields = ('created_at',   )

Explicacion:

#1. Se importa el modelo que hicimos

#2. Desde aqui Django va a saber que responder
cuando se haga una peticion "POS" "GET" "PUT" "DELETE".

#3. Esto es lo que va a combertir un modelo en datos que van a ser consultados.

#4. Proporciona metadatos sobre el modelo.

#5. Campos que quiero que sean consultados. 

#6. Campos de solo lectura osea que no se editan.


"View Set"  ==  Permite establecer quien puede consultar esto que peticiones se pueden hacer,
o si que hay que enviar una serie de autenticacion para poder acceder.

          NOTA: Dentro de la aplicacion, crear un archivo que se llame api.py

"api.py"

        from .models import (NOMBRE)
        from rest_framework import ViewSets, permissions
        from .serializers import ProjectSerializer


       #1.  class ProjectViewSet(viewsets.ModelViewSet):
       #2.    queryset = project.objects.all()
       #3.    permission_classes = [permissions.AllowAny]
           serializer_class = ProjectSerializer


  Explicacion:

#1. Aqui le vamos a decir que consultas se van a poder hacer.

#2. Consulta todos los datos.

#3. Cualquier cliente va a poder solicitar datos al servidor.


CREACION DE RUTAS MAS FACIL:

Aplicacion / Rutas 

     from rest_framework import routers
     from api import ProjectViewSet

	   router = routers.DefaultRouter()

     router.register('api/projects', ProjectViewSet, 'projects')

     urlpatterns = router.urls  // EL GENERA LAS "URLs"


AHORA EN EL PRJECTO VE A LAS "URLs.py"

	from django.contrib import admin
      from django.urls import path, include

      urlpatterns = [
      path('admin/', admin.site.urls),
      path('', include('projects.urls')),
      ] 
