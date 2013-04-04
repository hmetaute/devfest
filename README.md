Manejo de inventario
====================

Un cliente le ha comunicado que tiene una necesidad de crear un manejo de inventario con control de acceso a usuarios.
El cliente tiene varios locales esparcidos por la ciudad en los que quiere controlar las entradas y salidas de productos y resulta difícil conectar las tiendas en una red local por la distancia entre ellas.
Se decide recomendarle al cliente proveer acceso a internet en cada tienda y que el acceso al software se haga por web; además se le propone que pague por la cantidad de usuarios y productos que registre en el inventario que soporta el sistema.

A continuación se muestra cómo responder a esta necesidad con rapidez para que el cliente reciba con prontitud una versión utilizable del programa de manejo de inventario.
Como entrega de la primera iteración se entrega una aplicación con funcionalidad rudimentaria pero que cumple con la funcionalidad que más prioridad tiene para el cliente.

Creando una nueva aplicación
---------------------------

Luego de seguir los [pasos para configurar rails](http://guides.rubyonrails.org/getting_started.html) se crea con ruby on rails una aplicación llamada app_inventario con el comando: ```rails new app_inventario -d postgresql```

¿Qué acabamos de hacer?
-----------------------
Creamos una aplicación ejecutable que está configurada para correr sobre la base de datos postgresql.
Esta configuración del generador se hace para que nuestra aplicación pueda correr sobre una de las [configuraciones estándar de Heroku](https://devcenter.heroku.com/articles/cedar)

¿Podemos desplegar?
-------------------
Hasta este momento tenemos una aplicación desplegable. Podemos hacer desde este momento el release 0 de la aplición de inventario.

Les presento a Heroku
---------------------
Para hacer nuestro primer despliegue debemos [configurar una cuenta gratuita en Heroku](https://id.heroku.com/signup) e [instalar el conjunto de herramientas de Heroku](https://toolbelt.heroku.com).
Luego de hacer esto, iniciamos la interaccion con heroku y relacionamos nuestra nueva cuenta con el comando ```heroku login```

Se nos solicitará el ingreso de nuestro correo y contraseña y estaremos listos para desplegar nuestra nueva aplicación.

Necesitamos control de versiones. Repositorio Git sobre Github
--------------------------------------------------------------
Debemos [instalar un cliente de git en el caso windows](https://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git) y [crear una cuenta en github](https://github.com/signup/free) para [acceder a un nuevo repositorio](https://help.github.com/articles/creating-a-new-repository).

Estamos listos, veamos algunos comandos para hacer el despliegue
---------------------------------------------------------------
En primer lugar, debemos inicializar un nuevo repositorio con el comando ```git init```.
Agregamos los archivos que generamos con ruby con el comando ```git add .```
Hacemos nuestro primer commit local con el comando ```git commit -m "Este es el mensaje de mi primer commit en git"```
Le hacemos saber a Heroku que vamos a enviar una nueva aplicación con el comando ```heroku create```.

!Lanzamiento¡ El primer despliegue
------------------------------------
Estamos listos para subir nuestra aplicación a Heroku. Corremos el comando ```git push heroku master``` que envía la aplicación que acabamos de crear a heroku para almacenamiento en control de versiones y despliegue en vivo en internet.

¿Dynos?
-------
Heroku maneja un concepto de unidad de procesamiento y memoria para nuestra aplicación llamada Dyno. Con el siguiente comando nos aseguramos de asignar uno de estos para que reciba, instale y corra nuestra nueva aplicación: ```heroku ps:scale web=1```.

Nuestra primera visita
----------------------
Podemos desde este momento verificar que nuestra aplicación ya está corriendo en internet y que la aplicación de Rails se creó correctamente con el comando ```heroku open``` que abre el navegador por defecto en el sitio raíz del proyecto.

Una dependencia para dar algo de estilo
---------------------------------------
Antes de comenzar a escribir código de negocio, quiero mostrar una librería que va a ayudarnos a darle algo de estilo a nuestra aplicación.

<b>Twitter Bootstrap</b>

Ryan bates tiene un excelente tutorial sobre esta gema.
<img width="180" height="35" src="http://oi49.tinypic.com/s5wn05.jpg"></img>
Instalacion de bootstrap para Rails en [railscasts](http://railscasts.org)

Vamos a agregar la dependencia a una librería de interfaz gráfica para que nuestra aplicación tenga un diseño base.
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

¿Scaffolds?
-----------
Rails utiliza un modelo de generacion de codigo llamado scaffold (Un scaffold vendría siendo en español el andamiaje que se construye por fuera de una construcción). Este generador permite acelerar el desarrollo de CRUDs.
Creamos el scaffold para un producto muy sencillo con la línea ```rails g scaffold product name:string price:decimal code:string amount:integer --skip-stylesheets```

Luego de esto, tendremos un modelo en la base de datos que debemos aplicar con el comando ```rake db:migrate``` 

Para encontrar más información sobre qué tipos de datos podemos configurar sobre un scaffold, podemos referirnos a [esta página](http://overooped.com/post/100354794/ruby-script-generate-scaffold-types)

Nota de configuración
----------------------
Si queremos trabajar localmente con sqlite3 y en Heroku con Postgres, debemos tener un bloque como el siguiente en el archivo <b>Gemfile</b>
```ruby
group :development do
    gem 'sqlite3'
    gem 'sqlite3-ruby'	   
end 
```
Además de esto, debemos tener en el archivo <b>configuration/database.yml</b>
```yaml
development:
  adapter: sqlite3
  database: db/development.sqlite3
  pool: 5
  timeout: 5000
```

No debemos versionar en git ninguno de estos archivos ya que son de configuración local

Probando lo que acabamos de hacer: Gratificación instantánea
------------------------------------------------------------
Luego de haber corrido correctamente la migración de la base de datos, podemos correr un servidor local y ver nuestra aplicación con el comando ```rails server```
Podemos ver lo que acabamos de hacer en la aplicacion luego de que la consola corra correctamente este comando en la dirección ```http://localhost:3000/products```
¡Ya tenemos un CRUD que persiste correctamente en la base de datos!

Volvemos a Bootstrap: instalación de los css de la aplicación
-------------------------------------------------------------
Para sacar provecho de la gema de estilos bootstrap, corremos este comando para que nuestra aplicación reciba los estilos para mejorar su presentación: ```rails g bootstrap:install``` 

Instalemos esto en heroku
-------------------------
Debemos hacer commit a heroku de todo lo que hicimos con el comando ````git push heroku master````
Luego de esto, corremos el cambio de base de datos en Heroku con el comando ````heroku run rake db:migrate````

Unos toques de estilo
---------------------
Vemos en nuestro navegador una aplicación muy sencilla con unos estilos básicos. Ahora démosle un poco más de estructura a la ventana principal del proyecto. Editamos el archivo <b>app/views/layouts/application.html.erb</b> y envolvemos el bloque que contiene la palabra yield de la siguiente manera:

´´´´rails g bootstrap:layout application fixed´´´´ Lo que agrega una distribución a nuestra aplicación.
´´´´rails g bootstrap:themed products -f´´´´ Lo que modifica el estilo de nuestro modelo de producto.
