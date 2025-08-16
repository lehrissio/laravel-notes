# Services

- Podemos criar uma pasta de serviços, que estão relacionados ao service container, que é basicamente uma classe que pode ser carregada em qualquer área da aplicação e executar os métodos que ela contém.

	`App/Services/Nome.php`

```
`App/Services/Operations.php`

<?php

namespace App\Services;
use Illuminate\Contracts\Encryption\DecryptException;
use Illuminate\Support\Facades\Crypt;

class Operations
{
    public static function decryptId($value)
    {
        // check if $value is encrypted
        try {
            $value = Crypt::decrypt($value);
        } catch (DecryptException $e) {
            return redirect()->route('home');
        }
        return $value;
    }
}
```

- Exemplo de uso em um controller

```
App/Http/Controllers/MainController.php

public function deleteNote($id)
    {
        $id = Operations::decryptId($id);
        echo "I'm deleting note with id = $id";
    }
}
```