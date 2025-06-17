# Workshop - Introdução ao Laravel

## O que é o Laravel?

O Laravel é um framework PHP de código aberto, que tem como objetivo facilitar o desenvolvimento de aplicações web. Ele segue o padrão MVC (Model-View-Controller) e oferece uma série de recursos e ferramentas que tornam o processo de desenvolvimento mais rápido e eficiente. Ele é conhecido por sua comunidade muito ativa que contribui com pacotes e extensões para o framework.

## O que é um framework?

Um framework é uma estrutura de software que fornece uma base para o desenvolvimento de aplicações. Ele oferece um conjunto de bibliotecas, ferramentas e convenções que ajudam os desenvolvedores a criar aplicações de forma mais rápida e organizada. Os frameworks geralmente seguem padrões de design e arquitetura, como MVC, o que facilita a manutenção e escalabilidade do código.

## O que é o padrão MVC?

O padrão MVC (Model-View-Controller) é uma arquitetura de software que separa a aplicação em três componentes principais:

-   **Model**: Representa a lógica de negócios e os dados da aplicação. Ele é responsável por interagir com o banco de dados e fornecer os dados necessários para a View.
-   **View**: É a camada de apresentação da aplicação. Ela é responsável por exibir os dados para o usuário e receber as interações do usuário.
-   **Controller**: É o intermediário entre o Model e a View. Ele recebe as requisições do usuário, processa os dados com a ajuda do Model e retorna a resposta para a View.
    Esse padrão ajuda a organizar o código e facilita a manutenção e escalabilidade da aplicação, pois cada componente tem uma responsabilidade específica.

## Como começar um projeto com Laravel?

Vamos utilizar o composer para criar um novo projeto Laravel. O composer é um gerenciador de dependências para PHP que facilita a instalação e atualização de bibliotecas e frameworks.
Para instalar o composer, siga as instruções no site oficial: https://getcomposer.org/download/
Após instalar o composer, você pode criar um novo projeto Laravel com o seguinte comando:

```bash
composer create-project laravel/laravel:^11.0 nome-do-projeto
```

Nesse material estamos utilizando a versão 11 do Laravel, caso você instale sem especificar a versão, o composer irá instalar a versão mais recente do Laravel.
Aqui é importante verificar se a lib do SQLite está instalada, vamos utilizar o SQLite para facilitar o desenvolvimento, já que não precisamos de um servidor de banco de dados separado.

## Projeto Laravel

### Estrutura de pastas

A estrutura de pastas do Laravel é organizada de forma a facilitar o desenvolvimento e a manutenção da aplicação. Aqui estão algumas das pastas mais importantes:

-   **app**: Contém o código da aplicação, incluindo os Models, Controllers e outros componentes.
-   **bootstrap**: Contém os arquivos de inicialização da aplicação.
-   **config**: Contém os arquivos de configuração da aplicação.
-   **database**: Contém os arquivos de migração e seeders do banco de dados.
-   **public**: Contém os arquivos públicos da aplicação, como CSS, JavaScript e imagens. É a pasta que será acessada pelo servidor web.
-   **resources**: Contém os arquivos de recursos da aplicação, como views, traduções e arquivos de linguagem.
-   **routes**: Contém os arquivos de rotas da aplicação. É onde você define as URLs e os Controllers que serão chamados.
-   **storage**: Contém os arquivos de armazenamento da aplicação, como logs, cache e arquivos gerados pela aplicação.
-   **tests**: Contém os testes da aplicação.
-   **vendor**: Contém as dependências do Composer. Essa pasta é gerada automaticamente quando você instala as dependências do projeto com o Composer.
-   **.env**: Arquivo de configuração do ambiente da aplicação. É onde você define as variáveis de ambiente, como as credenciais do banco de dados e outras configurações específicas do ambiente.
-   **artisan**: É o console do Laravel. Ele fornece uma série de comandos que facilitam o desenvolvimento, como a criação de Controllers, Models e Migrations.
-   **composer.json**: Arquivo de configuração do Composer. É onde você define as dependências do projeto e outras configurações do Composer.
-   **package.json**: Arquivo de configuração do NPM. É onde você define as dependências do projeto e outras configurações do NPM.

### Configurando a autenticação utilizando um kit

O Laravel oferece um kit de autenticação pronto para uso, que facilita a implementação de autenticação em sua aplicação. Para instalar o kit de autenticação, execute o seguinte comando:

```bash
composer require laravel/breeze --dev
```

Após instalar o kit de autenticação, você pode executar o seguinte comando para instalar os arquivos de autenticação:

```bash
php artisan breeze:install
```

Aqui vamos utilizar o Blade com Alpine JS, mas você pode escolher o kit de autenticação que preferir.

Após instalar os arquivos de autenticação, você pode executar o seguinte comando para compilar os arquivos CSS e JavaScript:

```bash
npm install
```

Após compilar os arquivos CSS e JavaScript, você pode executar o seguinte comando para criar as tabelas de autenticação no banco de dados:

```bash
php artisan migrate
```

O comando `php artisan migrate` executa todas as migrations pendentes, criando as tabelas necessárias no banco de dados.

### Testando o projeto utilizando o artisan serve

Após criar o projeto, você pode iniciar o servidor embutido do Laravel com o seguinte comando:

```bash
php artisan serve
```

Um passo interessante para garantir que os estilos vão funcionar corretamente é executar o comando `npm run dev` durante o desenvolvimento. Esse comando compila os arquivos CSS e JavaScript e os coloca na pasta public, onde o Laravel pode acessá-los.

```bash
npm run dev
```

Com esses dois passos teremos um servidor na porta 8000 por padrão. Você pode acessar a aplicação no navegador através do seguinte endereço: http://localhost

### Criando um Model com uma migration

Vamos começar uma aplicação de lista de tarefas (to-do list) utilizando o Laravel. Para isso, vamos criar um Model chamado Task e uma migration para criar a tabela tasks no banco de
dados.

Para criar um Model com uma migration, você pode executar o seguinte comando:

```bash
php artisan make:model Task -m
```

Esse comando criará um Model chamado Task na pasta app/Models e uma migration na pasta database/migrations. A migration será responsável por criar a tabela tasks no banco de dados.
Abra o arquivo da migration e adicione os campos que você deseja na tabela tasks. Aqui está um exemplo de como a migration pode ficar:

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('tasks', function (Blueprint $table) {
            $table->id();
            $table->string('title'); // Título da tarefa - o string é o tipo de dado
            $table->foreignIdFor(\App\Models\User::class, 'user_id')->constrained()->onDelete('cascade'); // ID do usuário que criou a tarefa - o foreignIdFor cria uma chave estrangeira para o usuário
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void // Método que desfaz a migration
    {
        // Aqui você pode desfazer a migration, removendo a tabela tasks
        // Isso é útil para reverter as alterações no banco de dados
    {
        Schema::dropIfExists('tasks');
    }
};
```

Após criar a migration, você pode executar o seguinte comando para criar a tabela tasks no banco de dados:

```bash
php artisan migrate
```

Vamos ajustar nosso Model também para que ele tenha os campos que queremos. Abra o arquivo app/Models/Task.php e adicione os seguintes campos:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Task extends Model
{
    protected $fillable = ['title', 'user_id']; // Campos que podem ser preenchidos em massa

    public function user() // Relação com o Model User
    {
        return $this->belongsTo(User::class); // Uma Task pertence a um User
    }
}
```

### Criando um Controller para gerenciar as Tasks

Para gerenciar as Tasks, vamos criar um Controller chamado TaskController. Você pode criar o Controller com o seguinte comando:

```bash
php artisan make:controller TaskController
```

Esse comando criará um Controller chamado TaskController na pasta app/Http/Controllers. Abra o arquivo app/Http/Controllers/TaskController.php e adicione os seguintes métodos:

```php
namespace App\Http\Controllers;
use App\Models\Task;

class TaskController extends Controller
{
    public function index()
    {
        $tasks = Task::all();
        return view('tasks.index', compact('tasks'));
    }

    public function create()
    {
        return view('tasks.create');
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'title' => 'required|string|max:255',
            'user_id' => 'required|exists:users,id',
        ]);

        $task = Task::create($validated);
        return redirect()->route('tasks.index');
    }

    public function edit(Task $task)
    {
        return view('tasks.edit', compact('task'));
    }

    public function update(Request $request, Task $task)
    {
        $validated = $request->validate([
            'title' => 'required|string|max:255',
            'user_id' => 'required|exists:users,id',
        ]);

        $task->update($validated);
        return redirect()->route('tasks.index');
    }

    public function destroy(Task $task)
    {
        $task->delete();
        return redirect()->route('tasks.index');
    }
}
```

Podemos ajustar para a rota utilizar o usuário autenticado ao invés de receber o user_id no formulário. Para isso, vamos ajustar o método `store` e `update` do Controller:

```php
public function store(Request $request)
{
    $validated = $request->validate([
        'title' => 'required|string|max:255',
    ]);

    $task = new Task($validated);
    $task->user_id = auth()->id(); // Define o usuário autenticado como o criador da tarefa
    $task->save();

    return redirect()->route('tasks.index');
}
public function update(Request $request, Task $task)
{
    $validated = $request->validate([
        'title' => 'required|string|max:255',
    ]);

    $task->title = $validated['title'];
    $task->save();

    return redirect()->route('tasks.index');
}
```

### Definindo as rotas para o TaskController

Para definir as rotas para o TaskController, você pode abrir o arquivo routes/web.php e adicionar as seguintes rotas:

```php
use App\Http\Controllers\TaskController;
Route::middleware(['auth'])->group(function () {
    Route::resource('tasks', TaskController::class);
});
```

Essas rotas irão mapear as URLs para os métodos do TaskController. O middleware `auth` garante que apenas usuários autenticados possam acessar essas rotas.

### Criando as Views para as Tasks

Vamos criar as views para listar, criar, editar e excluir as Tasks. As views serão criadas na pasta resources/views/tasks.

Crie a pasta `tasks` dentro de `resources/views` e adicione os seguintes arquivos:

-   **index.blade.php**: Para listar as Tasks.
-   **create.blade.php**: Para criar uma nova Task.
-   **edit.blade.php**: Para editar uma Task existente.

#### index.blade.php

```blade
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Lista de Tasks') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6">
                    <h3 class="font-semibold text-lg mb-4">Tasks</h3>
                    <table class="min-w-full">
                        <thead>
                            <tr>
                                <th class="px-4 py-2">Título</th>
                                <th class="px-4 py-2">Ações</th>
                            </tr>
                        </thead>
                        <tbody>
                            @foreach ($tasks as $task)
                                <tr>
                                    <td class="border px-4 py-2">{{ $task->title }}</td>
                                    <td class="border px-4 py-2">
                                        <a href="{{ route('tasks.edit', $task) }}" class="text-blue-500">Editar</a>
                                        <form action="{{ route('tasks.destroy', $task) }}" method="POST" class="inline">
                                            @csrf
                                            @method('DELETE')
                                            <button type="submit" class="text-red-500">Excluir</button>
                                        </form>
                                    </td>
                                </tr>
                            @endforeach
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

#### create.blade.php

```blade
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Criar Task') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6">
                    <form action="{{ route('tasks.store') }}" method="POST">
                        @csrf
                        <div class="mb-4">
                            <label for="title" class="block text-gray-700">Título</label>
                            <input type="text" name="title" id="title" class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:border-blue-500 focus:ring-blue-500" required>
                        </div>
                        <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Salvar</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

#### edit.blade.php

```blade
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Editar Task') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6">
                    <form action="{{ route('tasks.update', $task) }}" method="POST">
                        @csrf
                        @method('PUT')
                        <div class="mb-4">
                            <label for="title" class="block text-gray-700">Título</label>
                            <input type="text" name="title" id="title" value="{{ $task->title }}" class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:border-blue-500 focus:ring-blue-500" required>
                        </div>
                        <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Salvar</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

### Deploy no Render utilizando Docker

Para fazer o deploy da sua aplicação Laravel no Render utilizando Docker, você precisa criar um arquivo `Dockerfile` e vamos criar um script de inicialização para o Laravel. O Render é uma plataforma de hospedagem que suporta aplicações Docker e facilita o deploy de aplicações web.

#### Dockerfile

```dockerfile
FROM richarvey/nginx-php-fpm:1.7.2Add commentMore actions

COPY . .

# Image config
ENV SKIP_COMPOSER 1
ENV WEBROOT /var/www/html/public
ENV PHP_ERRORS_STDERR 1
ENV RUN_SCRIPTS 1
ENV REAL_IP_HEADER 1

# Laravel config
ENV APP_ENV production
ENV APP_DEBUG false
ENV LOG_CHANNEL stderr

# Allow composer to run as root
ENV COMPOSER_ALLOW_SUPERUSER 1
CMD ["/start.sh"]
```

#### start.sh e nginx-site.conf

Vamos criar um script de inicialização chamado `start.sh` que será responsável por instalar as dependências do Composer, configurar o Laravel e executar as migrações. Além disso, vamos configurar o Nginx para servir a aplicação Laravel.

```bash
#!/usr/bin/env bash
echo "Running composer"
composer global require hirak/prestissimo
composer install --no-dev --working-dir=/var/www/html

echo "Caching config..."
php artisan config:cache

echo "Caching routes..."
php artisan route:cache

echo "Running migrations..."
php artisan migrate --force
```

Agora vamos criar a pasta chamada `conf/nginx` e dentro dela criar um arquivo `nginx-site.conf` com a seguinte configuração:

```nginx
server {
  # Render provisions and terminates SSL
  listen 80;

  # Make site accessible from http://localhost/
  server_name _;
  root /var/www/html/public;
  index index.html index.htm index.php;

  # Disable sendfile as per https://docs.vagrantup.com/v2/synced-folders/virtualbox.html
  sendfile off;

  # Add stdout logging
  error_log /dev/stdout info;
  access_log /dev/stdout;

  # block access to sensitive information about git
  location /.git {
    deny all;
    return 403;
  }

  add_header X-Frame-Options "SAMEORIGIN";
  add_header X-XSS-Protection "1; mode=block";
  add_header X-Content-Type-Options "nosniff";
  charset utf-8;

  location / {
      try_files $uri $uri/ /index.php?$query_string;
  }

  location = /favicon.ico { access_log off; log_not_found off; }
  location = /robots.txt  { access_log off; log_not_found off; }

  error_page 404 /index.php;

  location ~* \.(jpg|jpeg|gif|png|css|js|ico|webp|tiff|ttf|svg)$ {
    expires 5d;
  }

  location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass unix:/var/run/php-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
    include fastcgi_params;
  }

  # deny access to . files
  location ~ /\. {
    log_not_found off;
    deny all;
  }

  location ~ /\.(?!well-known).* {
    deny all;
  }
}
```

#### Configurando o Render

No Render vamos criar um novo serviço do tipo "Web Service" e escolher a opção "Docker". Precisamos adicionar as seguintes variáveis de ambiente:

-   `APP_KEY`: Chave de criptografia do Laravel. Você pode gerar uma nova chave com o comando `php artisan key:generate` localmente e copiar o valor gerado.
-   `DB_CONNECTION`: Tipo de conexão com o banco de dados. No nosso caso, será `sqlite`.
-   `DB_DATABASE`: Caminho para o arquivo do banco de dados SQLite. Por exemplo, `database/database.sqlite`.
-   `DB_FOREIGN_KEYS`: Deve ser definido como `true` para habilitar as chaves estrangeiras no SQLite.
-   `APP_ENV`: Defina como `production` para indicar que a aplicação está em produção.
-   `APP_DEBUG`: Defina como `false` para desativar o modo de depuração em produção.
-   `APP_URL`: URL da sua aplicação. Por exemplo, `https://sua-aplicacao.onrender.com`.
-   `ASSET_URL`: URL base para os assets da sua aplicação. Por exemplo, `https://sua-aplicacao.onrender.com`.
