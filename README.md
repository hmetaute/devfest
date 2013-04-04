Manejo de inventario
====================

Un cliente le ha comunicado que tiene una necesidad de crear un manejo de inventario con control de acceso a usuarios.
El cliente tiene varios locales esparcidos por la ciudad en los que quiere controlar las entradas y salidas de productos y resulta dif�cil conectar las tiendas en una red local por la distancia entre ellas.
Se decide recomendarle al cliente proveer acceso a internet en cada tienda y que el acceso al software se haga por web; adem�s se le propone que pague por la cantidad de usuarios y productos que registre en el inventario que soporta el sistema.

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
Para hacer nuestro primer despliegue debemos [configurar una cuenta gratuita en Heroku](https://id.heroku.com/signup) e [instalar el conjunto de herramientas de Heroku](https://toolbelt.heroku.com).
Luego de hacer esto, iniciamos la interaccion con heroku y relacionamos nuestra nueva cuenta con el comando ```heroku login```

Se nos solicitar� el ingreso de nuestro correo y contrase�a y estaremos listos para desplegar nuestra nueva aplicaci�n.

Necesitamos control de versiones. Repositorio Git sobre Github
--------------------------------------------------------------
Debemos [instalar un cliente de git en el caso windows](https://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git) y [crear una cuenta en github](https://github.com/signup/free) para [acceder a un nuevo repositorio](https://help.github.com/articles/creating-a-new-repository).

Estamos listos, veamos algunos comandos para hacer el despliegue
---------------------------------------------------------------
En primer lugar, debemos inicializar un nuevo repositorio con el comando ```git init```.
Agregamos los archivos que generamos con ruby con el comando ```git add .```
Hacemos nuestro primer commit local con el comando ```git commit -m "Este es el mensaje de mi primer commit en git"```
Le hacemos saber a Heroku que vamos a enviar una nueva aplicaci�n con el comando ```heroku create```.

!Lanzamiento� El primer despliegue
------------------------------------
Estamos listos para subir nuestra aplicaci�n a Heroku. Corremos el comando ```git push heroku master``` que env�a la aplicaci�n que acabamos de crear a heroku para almacenamiento en control de versiones y despliegue en vivo en internet.

�Dynos?
-------
Heroku maneja un concepto de unidad de procesamiento y memoria para nuestra aplicaci�n llamada Dyno. Con el siguiente comando nos aseguramos de asignar uno de estos para que reciba, instale y corra nuestra nueva aplicaci�n: ```heroku ps:scale web=1```.

Nuestra primera visita
----------------------
Podemos desde este momento verificar que nuestra aplicaci�n ya est� corriendo en internet y que la aplicaci�n de Rails se cre� correctamente con el comando ```heroku open``` que abre el navegador por defecto en el sitio ra�z del proyecto.

Una dependencia para dar algo de estilo
---------------------------------------
Antes de comenzar a escribir c�digo de negocio, quiero mostrar una librer�a que va a ayudarnos a darle algo de estilo a nuestra aplicaci�n.

<b>Twitter Bootstrap</b>

Ryan bates tiene un excelente tutorial sobre esta gema.
<img width="180" height="35" src="http://oi49.tinypic.com/s5wn05.jpg"></img>
Instalacion de bootstrap para Rails en [railscasts](http://railscasts.org)

Vamos a agregar la dependencia a una librer�a de interfaz gr�fica para que nuestra aplicaci�n tenga un dise�o base.
En este caso, queremos usar la ayuda de [twitter bootstrap](http://twitter.github.com/bootstrap/) y agregamos las funciones que nos provee para interfaz modificando el archivo en el que se manejan las dependencias: <b>Gemfile</b>
```ruby 
group :assets do
  gem 'less-rails'
  gem 'sass-rails',   '~> 3.2.3'
  gem 'coffee-rails', '~> 3.2.1'
  gem 'twitter-bootstrap-rails'  
  gem 'uglifier', '>= 1.0.3'
end
``` 

Y luego bajamos localmente la dependencia con el comando ```bundle install```

Ahora a crear valor de negocio
-------------------------------
Con la prueba de que todos los sistemas funcionan correctamente, podemos hacer un primer bosquejo de la funcionalidad de administrar productos en un inventario.

�Scaffolds?
-----------
Rails utiliza un modelo de generacion de codigo llamado scaffold (Un scaffold vendr�a siendo en espa�ol el andamiaje que se construye por fuera de una construcci�n). Este generador permite acelerar el desarrollo de CRUDs.
Creamos el scaffold para un producto muy sencillo con la l�nea ```rails g scaffold product name:string price:decimal code:string amount:integer --skip-stylesheets```

Luego de esto, tendremos un modelo en la base de datos que debemos aplicar con el comando ```rake db:migrate``` 

Para encontrar m�s informaci�n sobre qu� tipos de datos podemos configurar sobre un scaffold, podemos referirnos a [esta p�gina](http://overooped.com/post/100354794/ruby-script-generate-scaffold-types)

Nota de configuraci�n
----------------------
Si queremos trabajar localmente con sqlite3 y en Heroku con Postgres, debemos tener un bloque como el siguiente en el archivo <b>Gemfile</b>
```ruby
group :development do
    gem 'sqlite3'
    gem 'sqlite3-ruby'	   
end 
```
Adem�s de esto, debemos tener en el archivo <b>configuration/database.yml</b>
```yaml
development:
  adapter: sqlite3
  database: db/development.sqlite3
  pool: 5
  timeout: 5000
```

No debemos versionar en git ninguno de estos archivos ya que son de configuraci�n local

Probando lo que acabamos de hacer: Gratificaci�n instant�nea
------------------------------------------------------------
Luego de haber corrido correctamente la migraci�n de la base de datos, podemos correr un servidor local y ver nuestra aplicaci�n con el comando ```rails server```
Podemos ver lo que acabamos de hacer en la aplicacion luego de que la consola corra correctamente este comando en la direcci�n ```http://localhost:3000/products```
�Ya tenemos un CRUD que persiste correctamente en la base de datos!

Volvemos a Bootstrap: instalaci�n de los css de la aplicaci�n
-------------------------------------------------------------
Para sacar provecho de la gema de estilos bootstrap, corremos este comando para que nuestra aplicaci�n reciba los estilos para mejorar su presentaci�n: ```rails g bootstrap:install``` 

Instalemos esto en heroku
-------------------------
Debemos hacer commit a heroku de todo lo que hicimos con el comando ````git push heroku master````
Luego de esto, corremos el cambio de base de datos en Heroku con el comando ````heroku run rake db:migrate````

Unos toques de estilo
---------------------
Vemos en nuestro navegador una aplicaci�n muy sencilla con unos estilos b�sicos. Ahora d�mosle un poco m�s de estructura a la ventana principal del proyecto. Editamos el archivo <b>app/views/layouts/application.html.erb</b> y envolvemos el bloque que contiene la palabra yield de la siguiente manera:

����rails g bootstrap:layout application fixed���� Lo que agrega una distribuci�n a nuestra aplicaci�n.
����rails g bootstrap:themed products -f���� Lo que modifica el estilo de nuestro modelo de producto.
