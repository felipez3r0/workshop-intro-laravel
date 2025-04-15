# Workshop - Introdução ao Laravel

## O que é o Laravel?

O Laravel é um framework PHP de código aberto, que tem como objetivo facilitar o desenvolvimento de aplicações web. Ele segue o padrão MVC (Model-View-Controller) e oferece uma série de recursos e ferramentas que tornam o processo de desenvolvimento mais rápido e eficiente. Ele é conhecido por sua comunidade muito ativa que contribui com pacotes e extensões para o framework.

## O que é um framework?

Um framework é uma estrutura de software que fornece uma base para o desenvolvimento de aplicações. Ele oferece um conjunto de bibliotecas, ferramentas e convenções que ajudam os desenvolvedores a criar aplicações de forma mais rápida e organizada. Os frameworks geralmente seguem padrões de design e arquitetura, como MVC, o que facilita a manutenção e escalabilidade do código.

## O que é o padrão MVC?

O padrão MVC (Model-View-Controller) é uma arquitetura de software que separa a aplicação em três componentes principais:
- **Model**: Representa a lógica de negócios e os dados da aplicação. Ele é responsável por interagir com o banco de dados e fornecer os dados necessários para a View.
- **View**: É a camada de apresentação da aplicação. Ela é responsável por exibir os dados para o usuário e receber as interações do usuário.
- **Controller**: É o intermediário entre o Model e a View. Ele recebe as requisições do usuário, processa os dados com a ajuda do Model e retorna a resposta para a View.
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
- **app**: Contém o código da aplicação, incluindo os Models, Controllers e outros componentes.
- **bootstrap**: Contém os arquivos de inicialização da aplicação.
- **config**: Contém os arquivos de configuração da aplicação.
- **database**: Contém os arquivos de migração e seeders do banco de dados.
- **public**: Contém os arquivos públicos da aplicação, como CSS, JavaScript e imagens. É a pasta que será acessada pelo servidor web.
- **resources**: Contém os arquivos de recursos da aplicação, como views, traduções e arquivos de linguagem.
- **routes**: Contém os arquivos de rotas da aplicação. É onde você define as URLs e os Controllers que serão chamados.
- **storage**: Contém os arquivos de armazenamento da aplicação, como logs, cache e arquivos gerados pela aplicação.
- **tests**: Contém os testes da aplicação.
- **vendor**: Contém as dependências do Composer. Essa pasta é gerada automaticamente quando você instala as dependências do projeto com o Composer.
- **.env**: Arquivo de configuração do ambiente da aplicação. É onde você define as variáveis de ambiente, como as credenciais do banco de dados e outras configurações específicas do ambiente.
- **artisan**: É o console do Laravel. Ele fornece uma série de comandos que facilitam o desenvolvimento, como a criação de Controllers, Models e Migrations.
- **composer.json**: Arquivo de configuração do Composer. É onde você define as dependências do projeto e outras configurações do Composer.
- **package.json**: Arquivo de configuração do NPM. É onde você define as dependências do projeto e outras configurações do NPM.

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
