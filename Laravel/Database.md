- Heidi SQL (MySQL)

## Conexão com o banco de dados

A melhor prática para configurar a conexão com o banco de dados seria pelo arquivo `env`, porém também é possível configurar pelo arquivo `config/database`. Mas pelo arquivo `env` sempre será melhor, porque desta forma é possível fazer essa configuração de forma diferente para cada desenvolvedor, com seu próprio usuário, etc.

- 1° criar o banco de dados + usuário e senha
- 2°:

~~~
.env

DB_CONNECTION=mysql
DB_HOST=127.0.0.1 # padrão
DB_PORT=3306 # padrão mysql
DB_DATABASE=laravel_notes # nome do banco
DB_USERNAME=user_laravel_notes # nome do usuário
DB_PASSWORD=5azuQ2Y3qeSu # senha
~~~

## Testar database

- acessar arquivo do controller;
- add use do **Facades/DB**;
- Add o  código no método do login, por ex.

~~~
app/Http/Controllers/Authcontroller.php

try {
	DB::connection()->getPdo();
	echo 'Connection is OK!';
	
} catch (\PDOException $e) {
	echo "Connection failed: " . $e->getMessage();
	
}

echo 'FIM';
~~~

Se alguma configuração estiver incorreta no`env`, aparecerá um erro de acesso negado.
