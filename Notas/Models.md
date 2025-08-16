# Models

- Está relacionado com as comunicações feitas com a base de dados
- Eloquent ORM (Object-Relational Mapping)
- Permite fazer uma abstração da escrita de querys puras em SQL
- É uma classe muito semelhante ao controller que vai estar relacionada a uma tabela específica do banco de dados

`php artisan make:model Name`

- O nome deve ser único, estar no singular e ter a primeira letra maiúscula
- Deve também ser igual ao nome da tabela no banco de dados

```
app/Models/User.php

namespace App\Models;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    //
}
```

### Testar model

- Para testar o model, pode-se usar o código abaixo no controller

```
app/Http/Controllers/AuthController.php

// retorna todos os usuários do banco de dados

$users = User::all()->toArray();
echo '<pre>';
print_r($users);
```

- ou

```
app/Http/Controllers/AuthController.php

// como instancia de objeto da classe model

$userModel = new User();
$user = $userModel->all()->toArray();
echo '<pre>';
print_r($users);
```
