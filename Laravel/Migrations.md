- Usada para criar tabelas no banco de dados

`php artisan make:migration create_name_table`

 - Cria um arquivo em `database/migrations`
 - O método `up()` cria as tabelas e o método `down()` remove
 
```
database/migrations/2025_08_14_232917_create_users_table.php

public function up(): void
{
	// cria tabela
	Schema::create('users', function (Blueprint $table) {

		// chave primária
		$table->id()->autoIncrement(); 
		
		// cria uma varchar tamanho 50 e pode ser nulo
		$table->string('username', 50)->nullable(); 

		$table->string('password', 200)->nullable();

		$table->dateTime('last_login')->nullable();
		
		// se refere a coluna created_at e updated_at
		$table->timestamps(); 
		
		// se refere a coluna deleted_at
		$table->softDeletes(); 

	});
}

public function down(): void
{
	// desfaz tabela
	Schema::dropIfExists('users');

}
```

## Criar tabela

`php artisan migrate`

- Irá procurar as migrations que ainda não forma executadas e criar as tabelas
- O Laravel irá criar uma tabela migrations no banco de dados para armazenar as informações dos processos de migração, nunca deverá ser alterada

## Alterar tabela

`php artisan migrate:rollback`

- Irá executar os métodos `down()`
- Irá voltar apenas uma etapa do processo (apenas a última alteração)