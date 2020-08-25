# Today I Learned by *Paola Santollo*

Ruby and Rails official documentation reading personal journal

## Week 1


### Thu 23, July 2020 *[Rspec]*
It is a tool that helps us test the details and the business logic, these tests can simulate the flow of the most complete users, such as buying an item. Rspec does not use a web controller, so it simulates a user's interactions with a real site.It also serves as a detailed description of how our code works.
Un ejemplo de una prueba rspec 
describe Factorial do
  it "does something" do
    # ...
  end
end
la siguiente espera que la clase factorial de un resultado de 120
describe Factorial do
  let(:calculator) { Factorial.new }
  it "finds the factorial of 5" do  
    expect(calculator.factorial_of(5)).to eq(120)
  end
end


### Fri 24, July 2020 *[Mapping]*
Es un metodo de Ruby que se puede usar con arrays, hashes and ranges, nos ayuda  atrasformar datos. Este metodfo no cambia el valor original, si se desea cambiar debe agregarse "!".  El uso de maps hace que nuestro codigo sea legible para otras personas sin tranformar los datos. 
Mapeo relacional de objetos (ORM): simplifica el uso de bases de datos en aplicaciones.
    Utilice objetos para mantener registros de la base de datos
        Una clase para cada tabla en la base de datos
        Los objetos de la clase corresponden a filas en la tabla.
        Los atributos de un objeto corresponden a columnas de la fila
    Gestione el movimiento de información entre objetos y la base de datos back-end.
    Gestione las relaciones entre tablas (uniones), conviértalas en estructuras de datos vinculadas


## Week 2

### Mon 27, July 2020 *[Rails]*
Rails es un framework para desarrollar aplicacione sweb escrito en Ruby. Nos permite escribir menos codigo aumentando nuestra productividad. Incluye la infraestructura necesaria para procesar envios de formularios o acceder a base de datos.La escrucctura de archivos de Rails existe para comodidad de los desarrolladores. Los activos como imagenes,a rchivos css y Js tienen sus propias carpetas. Para crear un nuevo proyecto se necesita tener instalado ruby, sqlite3, node.js y yarn. Con el comando "rails new blog" se generan los archivos de nuestro proyecto. Para iniciar nuestro servidor web escribimos en la terminal "rails server".


### Tue 28, July 2020 *[Usefulo Rails comands]*
rails foo      
Creates a new Rails applications in subdirectory foo.

ruby script/generate model foo      
Creates a new model class Foo with a skeleton class definition in app/models/foo.rb and a skeletal migration in db/migrate/*_create_foos.rb.

ruby script/generate controller foo a b
Creates a new controller class FooController with a skeleton class definition in app/controllers/foo_controller.rb. It also creates skeleton action methods a and b in the controller, plus skeleton views in the files app/views/foo/a.html.erb and app/views/foo/a.html.erb. If a and b are omitted then the controller is created with no actions or views.

ruby script/generate migration foo
Creates a new migration in the file db/migrate/*_foo.rb.

rake db:migrate
Runs all migrations to bring the database up to date.

rake db:migrate:reset
Drops the database, creates a new one, and runs all migrations to bring the database up to date.

rake db:migrate VERSION=20090909201633_create_photos.rb
Runs migrations (either forward or backward) to restore the database to match the state just after the given migration.

ruby script/server
Starts up a Web server on port 3000 running the current application; log messages from the server will appear on standard output.

gem update
Updates all Ruby Gems.

gem update rails--include-dependencies
Updates Rails to the latest version, including all Gems that Rails depends upon.

gem update --system
Updates Ruby to the latest version.

https://web.stanford.edu/~ouster/cgi-bin/cs142-fall10/railsCommands.php


### Wed 29, July 2020 *[Setting Application Home Page]*
Podemos ver como se ve nuestra pagina en localhost:3000. Primero devemos ver donde esta localizada nuestra pagina de inicio, esto lo vemos en nuestro archivo de enrutamiento config/routes.rb . Si se ejecuta "rails routes" se veran todas las acciones restful estandar de nuestro proyecto. 
Despues de que definimos nuestras routes, se necesita tener un controlador para atender la solicitud, para crear uno ejecutamos "rails generate controller articles"(articles es el nombre de nuestro controlador), el cual creara nuestro archivo controlador en la carpeta app/controllers/articles_controller.rb este se encontrara vacio, dentro de el se definen los metodos que despues se convertiran en acciones. Estas realizaran operaciones CRUD en los articulos de nuestro sistema. Posteriomente de crear nuestro controlador procedemos a crear nuestra vista en la carpeta "app/views/articles/new.html.erb".


### Thu 30, July 2020 *[Rails Architecture]*
El objectivo principal de Rails es liberar al desarrolador de problemas tecologicos y concentrase en resolver problemas comerciales. Se usa el modelo MVC (Model Views Controllers), esto con el fin de aumentar el rendimiento de la aplicacion.
Model: es donde se encuntra la logica empresarial y las reglas para modificar los datos.Es un compoonente importante de nuestro negocio pues almacena nuestra informacion.
Views: Mantiene la logica de la interfaz, se usa html que contiene codigo ruby muy simple con bucles y condiciiones.Dependen de los datos del controlador.
Controller: maneja el flujo de la aplicacion, es el que se comunica con las views and models 

Server: es nuestro servidor web ejecutable. El servidor de Rails utiliza arquitectura MVC, recibe la solicitud y asigna el procesamiento a los diferentes componentes.
Routes: Indican que URL dirige nuestra applicacion y que parte del codigo gestiona las solicitudes relacionadas.
Assents: es el codigo css, js y otros medios como imagenes.


### Fri 31, July 2020 *[Forms]*
Se crean en la carpeta app/views/..
Ejemplo, formulario para agregar un nuevo articulo:
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
<% end %>
Para que funcione correctamente debemos definir en nuestro controlador el metodo "create"
El metodo render toma un hash muy simple con una clave :plain y un valor de params[:article].inspect
 

## Week 2

### Mon 3, August 2020 *[Creating the model ]*
Podemos crearlo desde nuestra terminal ejecutando "rails generate model Article title:string text:text" con esto decimos que queremos el modelo Articulo, con un atributte "title" type string and "text" type text.
Estos atributos se agregan en automatico , creando la estructura de la base de datos.
Despues de crear nuestra tabla ejecutamos el comando de migracion "rails db:migrate". Si desea ejecutar las migraciones en otro entorno, por ejemplo, en la producción, se debe pasar de forma explícita cuando se invoca el comando: rails db:migrate RAILS_ENV=production

### Tue 4, August 2020 *[Saving data]*
Despues de agregar el modleo, vamos a nuestro controlador para modificar nuestro metodo "create" 
def create
  @article = Article.new(article_params)
 
  @article.save
  redirect_to @article
end
 
private
  def article_params
    params.require(:article).permit(:title, :text)
  end
Tenemos que definir nuestros parametros de controlador permitidos para evitar una asignacion masiva incorrecta , a menudo el metodo se hace private 
### Wed 5, August 2020 *[Showing and listing]*
Para ver nuestros articulos primero agregamos la route 
"article GET  /articles/:id(.:format)  articles#show" y despues modificamos el controlador de articulos y agregamos una vista "show.html.erb"
Para ver la lista de los articulos agregamos primero en nuestra ruta
"articles GET  /articles(.:format)  articles#index " y en nuestro controlador de articulos 
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
 
  def show
    @article = Article.find(params[:id])
  end
 
  def new
  end


### Thu 6, August 2020 *[Adding links]*
Modificamos nuestro archivo index.html.erb
Ejemplo: 
<h1>Hello, Rails!</h1>
<%= link_to 'My Blog', controller: 'articles' %>
Para agregar un nuevo articulo:
<%= link_to 'New article', new_article_path %>


### Fri 7, August 2020 *[Validation]*
Rails tiene metodos para validar los datos que enviamos a nuestros modelos, los podemos modificar en nuestro archivo que se encuentra en app/models/, podmeos agregarle "presence: true, length: { minimum: 5 }" entre otras validaciones 
tambien debemos modificar nuestro controlador y vista.
def new
  @article = Article.new
end
 
def create
  @article = Article.new(article_params)
 
  if @article.save
    redirect_to @article
  else
    render 'new'
  end
end
 
private
  def article_params
    params.require(:article).permit(:title, :text)
  end


## Week 3

### Mon 10, August 2020 *[Active Record Validations]*
Se usan para validar que los datos que se guarden sean los correctos. Hacerlas desde la base de datos puede dificultar las pruebas, desde el controlador pueden ser dificiles de mnaejar, probar y mantener. Active Record utiliza el new_record? método de instancia para determinar si un objeto ya está en la base de datos o no.
Los siguientes métodos activan validaciones y guardarán el objeto en la base de datos solo si el objeto es válido:
create, create!, save, save!, update, update!.
Los siguientes métodos omiten las validaciones y guardarán el objeto en la base de datos independientemente de su validez: decrement!, decrement_counter, increment!, increment_counter, toggle!, touch, update_all, update_attribute, update_column, update_columns, update_counters.
valid? y invalid?; Antes de guardar un objeto de registro activo, Rails ejecuta sus validaciones. Si estas validaciones producen algún error, Rails no guarda el objeto.
errors[]; Para verificar si un atributo particular de un objeto es válido, puede usar errors[:attribute]. Devuelve una matriz de todos los errores de :attribute. Si no hay errores en el atributo especificado, se devuelve una matriz vacía.
errors.details; Para comprobar qué validaciones fallaron en un atributo no válido, puede utilizar errors.details[:attribute]. Devuelve una matriz de hashes con una :error clave para obtener el símbolo del validador.



### Tue 11, August 2020 *[Using partials]*


### Wed 12, August 2020 *[Delete data]*


### Thu 13, August 2020 *[Associating models]*


### Fri 14, August 2020 *[Refactoring]*




## Week 4

### Mon 17, August 2020 *[Security]*

### Tue 18, August 2020 *[Cookies and Sessions]*


### Wed 19, August 2020 *[Validation helpers]*
Los ayudantes d evalidacion proporcionan reglas ocmunes. Cuando falla una validacion se agrega un mensjae a la errors y este mensaje se asocia con el atributo que se esta validando.
Acceptance: valida que se marcó una casilla de verificación en la interfaz de usuario cuando se envió un formulario. Esto se usa generalmente cuando el usuario necesita aceptar los términos de servicio de su aplicación, confirmar que se lee algún texto o cualquier concepto similar.
Validates_associated: cuando su modelo tenga asociaciones con otros modelos y también necesiten ser validados. Cuando intente guardar su objeto, valid? se llamará a cada uno de los objetos asociados.
Confirmation: se utiliza cuando se tienen dos campos de texto que deben recibir exactamente el mismo contenido. Por ejemplo, es posible que desee confirmar una dirección de correo electrónico o una contraseña. Esta validación crea un atributo virtual cuyo nombre es el nombre del campo que debe ser confirmado con "_confirmation" adjunto.
Exclusion: valida que los valores de los atributos no estén incluidos en un conjunto dado. De hecho, este conjunto puede ser cualquier objeto enumerable.
Format: valida los valores de los atributos probando si coinciden con una expresión regular determinada, que se especifica mediante la :withopción.
Inclusion: valida que los valores de los atributos estén incluidos en un conjunto dado. De hecho, este conjunto puede ser cualquier objeto enumerable.
Length: valida la longitud de los valores de los atributos. 
Numericality: valida que sus atributos solo tengan valores numéricos. De forma predeterminada, coincidirá con un signo opcional seguido de un número de punto flotante o integral. Para especificar que solo se permiten números enteros, establézcalo :only_integeren verdadero.
Presence: valida que los atributos especificados no estén vacíos. Utiliza el blank?método para verificar si el valor es niluna cadena en blanco, es decir, una cadena que está vacía o consta de espacios en blanco.
Absence: Este ayudante valida que los atributos especificados estén ausentes. Utiliza el present?método para verificar si el valor no es nulo o una cadena en blanco, es decir, una cadena que está vacía o consta de espacios en blanco.
Uniqueness: Esta validacion ocurre al realizar una consulta SQL en la tabla del modelo, buscando un registro existente con el mismo valor en ese atributo. Existe una :scopeopción que puede utilizar para especificar uno o más atributos que se utilizan para limitar la comprobación de uniqueness:
Validates_with: El validates_withayudante toma una clase o una lista de clases para usar para la validación. No hay un mensaje de error predeterminado para validates_with. Debemos agregar errores manualmente a la colección de errores del registro en la clase de validación. Para implementar el método de validación, debe tener recorddefinido un parámetro, que es el registro a validar. Al igual que todas las demás validaciones, validates_withtoma los :if, :unlessy :onopciones. Si pasa cualquier otra opción, enviará esas opciones a la clase de validación como options.Tenemos que tener en cuenta que solo se inicializará solo una vez durante todo el ciclo de vida de la aplicación, y no en cada ejecución de validación, así que tenga cuidado con el uso de variables de instancia dentro de él.
Validates_each: Este ayudante valida los atributos contra un bloque. No tiene una función de validación predefinida. Debe crear uno usando un bloque, y cada atributo que se le pase validates_eachse probará con él. En el siguiente ejemplo, no queremos que los nombres y apellidos comiencen con minúsculas.
class Person < ApplicationRecord
  validates_each :name, :surname do |record, attr, value|
    record.errors.add(attr, 'must start with upper case') if value =~ /\A[[:lower:]]/
  end
end



### Thu 20, August 2020 *[Common validation options]*
:allow_nil
La :allow_nilopción omite la validación cuando el valor que se valida es nil.
:allow_blank
La :allow_blankopción es similar a la :allow_nilopción. Esta opción permitirá que pase la validación si el valor del atributo es blank?, como nilo una cadena vacía.
:message Nos permite especificar el mensaje que se agregará a la errorscolección cuando falle la validación. Cuando no se usa esta opción, Active Record usará el mensaje de error predeterminado respectivo para cada ayudante de validación
:on
Permite especificar cuándo debe ocurrir la validación. El comportamiento predeterminado para todos los ayudantes de validación integrados es ejecutar al guardar (tanto cuando está creando un nuevo registro como cuando lo está actualizando).
Validaciones estrictas: También puede especificar que las validaciones sean estrictas y que se generen ActiveModel::StrictValidationFailed cuando el objeto no es válido.



### Fri 21, August 2020 *[Conditional validations]*
A veces, tendrá sentido validar un objeto solo cuando se satisfaga un predicado determinado. Puede hacerlo utilizando las opciones :ify :unless, que pueden tomar un símbolo, una Proco una Array. Puede utilizar la :if opción cuando desee especificar cuándo debe realizarse la validación . Si desea especificar cuándo no debe ocurrir la validación , puede usar la :unlessopción.
Puede asociar las opciones :ify :unlesscon un símbolo correspondiente al nombre de un método que se llamará justo antes de que ocurra la validación. 
Es posible asociar :ify :unlesscon un Procobjeto que será llamado. El uso de un Procobjeto le brinda la posibilidad de escribir una condición en línea en lugar de un método separado.
Por otro lado, cuando varias condiciones definen si debe ocurrir una validación o no, Arrayse puede utilizar un. Además, puede aplicar ambos :ify :unlessa la misma validación.



## Week 5

### Mon 24, August 2020 *[Custom validations]*
Los validadores personalizados son clases que heredan ActiveModel::Validator. Estas clases deben implementar el validatemétodo que toma un registro como argumento y realiza la validación en él. El validador personalizado se llama mediante el validates_with método.
La forma más fácil de agregar validadores personalizados para validar atributos individuales es con la conveniente ActiveModel::EachValidator. En este caso, la clase de validación personalizada debe implementar un validate_eachmétodo que tome tres argumentos: registro, atributo y valor. Estos corresponden a la instancia, el atributo a validar y el valor del atributo en la instancia pasada.
class EmailValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    unless value =~ /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i
      record.errors[attribute] << (options[:message] || "is not an email")
    end
  end
end
 
class Person < ApplicationRecord
  validates :email, presence: true, email: true
end




### Tue 25, August 2020 *[Validation errors]*
Ademas de valid? y invalid? existen mas metodos que nos ayudan a trabajar con errors.
errors: Devuelve una instancia de la clase que ActiveModel::Errorscontiene todos los errores. Cada clave es el nombre del atributo y el valor es una matriz de cadenas con todos los errores.
errors[]: se utiliza cuando desea comprobar los mensajes de error de un atributo específico. Devuelve una matriz de cadenas con todos los mensajes de error para el atributo dado, cada cadena con un mensaje de error. 
errors.add: Permite agregar un mensaje de error relacionado con un atributo en particular. Toma como argumentos el atributo y el mensaje de error.
errors.details: Especifica un tipo de validador para el hash de detalles del error devuelto utilizando el error.
errors[:base]; Puede agregar mensajes de error relacionados con el estado del objeto como un todo, en lugar de estar relacionados con un atributo específico. 
errors.clear; Se usa cuando intencionalmente desea borrar todos los mensajes de la errorscolección. Por supuesto, llamar errors.cleara un objeto no válido no lo hará válido: la errorscolección ahora estará vacía, pero la próxima vez que llame valid?o cualquier método que intente guardar este objeto en la base de datos, las validaciones se ejecutarán nuevamente. 
errors.size: Devuelve el número total de mensajes de error del objeto.



### Wed 26, August 2020 *[Validations errors in views]*
Una vez que haya creado un modelo y haya agregado validaciones, si ese modelo se crea a través de un formulario web, probablemente desee mostrar un mensaje de error cuando una de las validaciones falle.
Suponiendo que tenemos un modelo que se ha guardado en una variable de instancia denominada @article, se ve así:

<% if @article.errors.any? %>
  <div id="error_explanation">
    <h2><%= pluralize(@article.errors.count, "error") %> prohibited this article from being saved:</h2>
 
    <ul>
    <% @article.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
Además, si utiliza los ayudantes de formulario de Rails para generar sus formularios, cuando se produce un error de validación en un campo, se generará un extra <div>alrededor de la entrada.

<div class="field_with_errors">
 <input id="article_title" name="article[title]" size="30" type="text" value="">
</div>


### Thu 27, August 2020 *[Scaffolding]*
Es un generador de codigo que nos ayuda realizar el CRUD de nuestra aplicacion de una manera sencilla.
Para implementarlo en nuestra terminal escribimos "rails generate scaffold user nombre: string email:string" esto suponiendo que se desea crear el moenlo de un usuario el cual tiene nombre y email.
Para iimplementar el post se escribe en la terminal "rails generate scaffold Post titulo:strig contenido:string user_id:integer".
El parametro "id" es creado automaticamente por lo cual no es necesario ponerlo.
Despues se actualiza nuestra base de batos "bundle exec rake db:migrate", con esto se catualizara nuestra database.
Despues de hacer nuestro podemos podemos visualizarlo en localhost:3000/users, si accedemos a localhost:3000/users/post mostratra la lista de todos los post y una pagina donde podemos crear nuevos.
Para crear una asociacion, es decir si un usuario tiene muchos post.
>>>
A user has many microposts.
app/models/user.rb
class User < ActiveRecord::Base
  has_many :microposts
end
>>>
A micropost belongs to a user.
app/models/micropost.rb
class Micropost < ActiveRecord::Base
  belongs_to :user
end
Podmeos ver el resultado de esta asociacion poniendo en nuestra termina "rails console", "first_user=user.first" con esto nos aparece la informacion del usuario, "first_user.microposts" para ver la relacion del usuario con otros post.


### Fri 28, August 2020 *[Reading topic or title]*
