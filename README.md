Manejo de inventario
====================

Un cliente le ha comunicado que tiene una necesidad de crear un manejo de inventario con control de acceso a usuarios.
El cliente tiene varios locales esparcidos por la ciudad en los que quiere controlar las entradas y salidas de productos y resulta dif�cil conectar las tiendas en una red local por la distancia entre ellas.
Se decide recomendarle al cliente proveer acceso a internet en cada tienda y que el acceso al software se haga por web y con la propuesta de que el cliente pague por la cantidad de usuarios y productos que registre en el inventario que soporta el sistema.

A continuaci�n se muestra c�mo responder a esta necesidad con rapidez para que el cliente reciba con prontitud una versi�n utilizable del programa de manejo de inventario.
Como entrega de la primera iteraci�n se entrega una aplicaci�n con funcionalidad rudimentaria pero que cumple con la funcionalidad que m�s prioridad tiene para el cliente.

Creando una nueva aplicaci�n
---------------------------

Luego de seguir los [pasos para configurar rails](http://guides.rubyonrails.org/getting_started.html) se crea con ruby on rails una aplicaci�n llamada app_inventario con el comando: ```rails new app_inventario -d postgresql```

�Qu� acabamos de hacer?
-----------------------
Creamos una aplicaci�n ejecutable que est� configurada para correr sobre la base de datos postgresql.
Esta configuraci�n del generador se hace para que nuestra aplicaci�n pueda correr sobre una de las [configuraciones est�ndar de Heroku](https://devcenter.heroku.com/articles/cedar)

�Podemos desplegar?
-------------------
Hasta este momento tenemos una aplicaci�n desplegable. Podemos hacer desde este momento el release 0 de la aplici�n de inventario.

Les presento a Heroku
---------------------
Para hacer nuestro primer despliegue debemos [configurar una cuenta gratuita en Heroku](https://id.heroku.com/signup) e [instalar el conjunto de herramientas de Heroku](https://toolbelt.heroku.com)