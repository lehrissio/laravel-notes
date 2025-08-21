# Controller

- cada controller deve ter um conjunto de métodos para uma funcionalidade específica da aplicação.
- para criar um controller:
	`php artisan make:controller MainController`

- métodos:

```
class MainController extends Controller
{
	public function index()
    {
        echo 'index';
    }

    public function about()
    {
        echo 'about';
    }
}
```

- type decoration -> definição do tipo de retorno no método

```
class MainController extends Controller
{
	public function initMethod(): string
    {
        return "Hello World!";
    }
    
    public function viewPage(): View
    {
        return view('home);
    }
}
```

## single action controller

`php artisan make:controller SingleActionController --invokable`

- deverá ter apenas um método público, podendo ter outro privados

```
class SingleActionController extends Controller
{
    public function __invoke(Request $request): void
    {
        echo "Single action Controller<br>";
        echo $this->privateMethod();
    }

    private function privateMethod(): string
    {
        return 'Private method';
    }
}
```

- declaração da rota

```
// route para controller single action
Route::get('/single', SingleActionController::class)->name('single);
```

## resource controller

`php artisan make:controller UserController --resource`

- cria um conjunto de metodos para acelerar o desenvolvimento do código

```
class UserController extends Controller
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        //
    }

    /**
     * Show the form for creating a new resource.
     */
    public function create()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
        //
    }


    /**
     * Display the specified resource.
     */
    public function show(string $id)
    {
        //
    }


    /**
     * Show the form for editing the specified resource.
     */
    public function edit(string $id)
    {
        //
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, string $id)
    {
        //
    }

    /**
     * Remove the specified resource from storage.
     */
    public function destroy(string $id)
    {
        //
    }
}
```

- criar rotas automaticamente para controller do tipo resource
```
// route para controller do tipo resource
Route::resource('users', UserController::class);

// ou para um conjunto de controllers
Route::resources([
    'users' => UserController::class,
    'clients' => ClientsController::class,
    'products' => ProductsController::class
]);
```

## controller.php

- o arquivo controller.php pode conter métodos que os outros controllers podem utilizar também, pois todos os outros controllers são extensões desse arquivo.

```
// exemplo controller.php

abstract class Controller
{
    public function cleanUpperCaseString($string): string
    {
        // remover os espaços em branco no início e fim de uma string
        // converter a string para uppercase (maiúsculas)
        return strtoupper(trim($string));
    }
}
```

```
// exemplo MainController

class MainController extends Controller
{
    public function teste($value): void{
        echo "A string final é: " . $this->cleanUpperCaseString($value);
    }
}
```