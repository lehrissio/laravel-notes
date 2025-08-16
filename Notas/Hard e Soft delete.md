# Hard delete

- Deleta fisicamente o dado

- Exemplo no método do delete:

```
App/Http/Controllers/MainController.php

public function deleteConfirm($id)
{
	// check if id is encrypted
	$id = Operations::decryptId($id);

	// load note
	$note = Note::find($id);
	
	// 1. hard delete
	$note->delete();

	return redirect()->route('home');
}
```

# Soft delete

- Uma maneira de não mostrar os dados na view sem apaga-los fisicamente
- Add coluna deleted_at na migration da tabela

```
database\migrations\2025_08_14_232917_create_users_table.php

public function up(): void
{
	Schema::create('users', function (Blueprint $table) {
		$table->timestamps(); // se refere a coluna created_at e updated_at
		$table->softDeletes(); // se refere a coluna deleted_at
	});
}
```

- Exemplo no método do delete:

```
App/Http/Controllers/MainController.php

public function deleteConfirm($id)
{
	// check if id is encrypted
	$id = Operations::decryptId($id);

	// load note
	$note = Note::find($id);

	// 2. soft delete
	$note->deleted_at = date('Y:m:d H:i:s');
	$note->save();

	// redirect to home
	return redirect()->route('home');
}
```

# Soft delete através do model

- Adicionar ao model:

```
app/Models/Note.php

use Illuminate\Database\Eloquent\SoftDeletes;

class Note extends Model
{
    use SoftDeletes;
    
    public function user(){
        return $this->belongTo(User::class);
    }
}
```

- Exemplo no método delete

```
App/Http/Controllers/MainController.php

public function deleteConfirm($id)
{
	// check if id is encrypted
	$id = Operations::decryptId($id);

	// load note
	$note = Note::find($id);
	
	// 3. soft delete (property in model)
	$note->delete();

	return redirect()->route('home');
}
```

# Hard delete quando esta usando a propriedade no model

```
App/Http/Controllers/MainController.php

public function deleteConfirm($id)
{
	// 4. hard delete (property in model)
	$note->forceDelete();
}
```

