# Eloquent Relationships

- As tabelas pedem se relacionar entre si
- Cada model do Eloquent (`use Illuminate\Database\Eloquent\Model`) representa uma tabela da base de dados
- O Eloquent consegue determinar automaticamente a chave estrangeira, desde que tenha esta morfologia:
	Ex. 1: `tabela1.id -> tabela2.tabela1_id`,
	Ex. 2: `user.id -> note.user_id`

## Como relacionar duas tabelas através dos Models

- A tabela User tem a relação de 1 para muitos com a tabela Notes, ou seja, um usuário pode ter diversas notas
- Exemplo de como fazer a vinculação entre as duas tabelas através das models:

```
app/Models/User.php

class User extends Model
{
    public function notes(){
        return $this->hasMany(Note::class);
    }
}
```

```
app/Models/Note.php

class Note extends Model
{
    public function user(){
        return $this->belongTo(User::class);
    }
}
```

## Como testar este relacionamento

- Adicionar no controller:

```
app/Http/Controllers/MainController.php

// irá armazenar na variável o id do usuário logado, capturado pela session no AuthController
$id = session('user.id');

// irá armazenar na variável os dados do usuário logado. através do Model User
$user = User::find($id)->toArray();

// irá armazenar na variável as notas do usuário logado, através do metódo notes() no model User
$notes = User::find($id)->notes()->get()->toArray();

// mostra na tela
echo '<pre>';
print_r($user);
print_r($notes);

// encerra a execução
die();
```