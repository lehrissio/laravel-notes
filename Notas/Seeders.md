# Seeders

- Usado para popular dados nas tabelas do banco de dados

- Criar seeder:
	`php artisan make:seeder NameSeeder`


```
database/seeders/UsersTableSeeder.php

namespace Database\Seeders;
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\DB; // precisa adicionar

class UsersTableSeeder extends Seeder
{
    public function run(): void
    {
        // criar usuarios
        DB::table('users')->insert([
            [
                'username' => 'user1@gmail.com',
                
                // encriptação da senha
                'password' =>bcrypt('abc123456'),
                
                // data aleatória
                'create_at' => date('Y-m-d H:i:s')
            ],
            [
                'username' => 'user2@gmail.com',
                'password' =>bcrypt('abc123456'),
                'create_at' => date('Y-m-d H:i:s')
            ],
            [
                'username' => 'user3@gmail.com',
                'password' =>bcrypt('abc123456'),
                'created_at' => date('Y-m-d H:i:s')
            ],
        ]);
    }
}
```

- Para executar seeder:
	`php artisan db:seed NameSeeder`