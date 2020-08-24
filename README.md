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


### Tue 11, August 2020 *[Using partials]*


### Wed 12, August 2020 *[Delete data]*


### Thu 13, August 2020 *[Associating models]*


### Fri 14, August 2020 *[Refactoring]*




## Week 4

### Mon 17, August 2020 *[Security]*

### Tue 18, August 2020 *[Cookies and Sessions]*


### Wed 19, August 2020 *[Validation helpers]*


### Thu 20, August 2020 *[Common validation options]*


### Fri 21, August 2020 *[Conditional validations]*




## Week 5

### Mon 24, August 2020 *[Custom validations]*

### Tue 25, August 2020 *[Validation errors]*


### Wed 26, August 2020 *[Validations errors in views]*


### Thu 27, August 2020 *[Scaffolding]*


### Fri 28, August 2020 *[Reading topic or title]*
