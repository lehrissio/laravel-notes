# Routes

- web.php -> rotas para web
- console.php -> rotas para console
- api.php -> rotas para api
- Na maior parte dos casos as rotas direcionam para controllers, mas podem direcionar para view e funções também.
- Declaração de uma rota:

```
routes/web.php

Route::get/post/out/patch('/enderecoDaPagina', [MainController::class, 'metodoDoController'])->name('nomeDaRota');`
```

- Exemplo de uso com o blade na view

```
resources/view/home.blade.php

<a href="{{ route('logout') }}" class="btn btn-outline-secondary px-3">
	Logout<i class="fa-solid fa-arrow-right-from-bracket ms-2"></i>
</a>
```

- outros exemplos

```
// função anônima
Route::get('/rota', function (){
	return <h1>Olá Laravel!<h1>;
})

// injeção
Route::get('/injection', function (Request $request){
	var_dump($request);
})

// aceita get e post
Route::match(['get', 'post'], '/injection', function (Request $request){
	return <h1>Aceita GET e POST<h1>;
})

// aceita qualquer verb
Route::any('/any', function (Request $request){
	return <h1>Aceita qualquer http verb<h1>;
})

// redirecionar rotas
Route::redirect('/saltar', '/index');
Route::permanentRedirect('/saltar2', '/index');

// view
Route::view('/view', 'home');
Route::view('/view2', 'home', ['myName' => 'Leticia']);
```

- verificar rotas pelo terminal
	`php artisan route:list -v`

## route parameters

- controlador:

```
class MainController extends Controller
{
	public function mostrarValor($valor)
	{
		echo "Valor enviado pela rota: $valor";
	}
	
	public function mostrarValores($valor1, $valor2)
	{
		echo "Valor enviado pela rota: $valor1 e $valor2";
	}
	
	public function mostrarValores2(Request $request, $valor1, $valor2)
	{
		echo "Valor enviado pela rota: $valor1 e $valor2";
	}
	
	public function mostrarValorOpcional($valor = null)
	{
		echo "Valor opcional: $valor";
	}
	
	public function mostrarValorOpcional2($valor1, $valor2 = 10)
	{
		echo "Valor enviado pela rota: $valor1 e $valor2";
	}
	
	public function mostrarPosts($user_id, $post_id)
	{
		echo "ID do usuário: $user_id e ID do post: $post_id";
	}
}
```

- exemplo de rotas:

```
Route::get('/valor/{value}', [MainController::class, 'mostrarValor']);
Route::get('/valores/{value1}/{value2}', [MainController::class, 'mostrarValores']);
Route::get('/valores2/{value1}/{value2}', [MainController::class, 'mostrarValores2']);

Route::get('/opcional/{value?}', [MainController::class, 'mostrarValorOpcional']);
Route::get('/opcional2/{value1}/{value2?}', [MainController::class, 'mostrarValorOpcional2']);

Route::get('/user/{user_id}/post/{post_id}', [MainController::class, 'mostrarPosts']);
```

## parâmetros de rotas com restrições

- exemplo de rota que aceita como parâmetro apenas números inteiros

```
// numeros inteiros
Route::get('/exp1/{value}', function($value){
    echo $value;
})->where('value', '[0-9]+'); // expressão regular
  
// strings alfanumericas
Route::get('/exp2/{value}', function($value){
    echo $value;
})->where('value', '[A-Za-z0-9]+');

// mais de um valor
Route::get('/exp3/{value1}/{value2}', function($value1, $value2){
    echo $value1 . "<br>";
    echo $value2;
})->where([
    'value1' => '[0-9]+',
    'value2' => '[A-Za-z0-9]+'
]);
```

## route names

- nome da rota para ser usado no código;
```
Route::get('/rota_abc', function() {
    return 'Rota nomeada';
})->name('rota_nomeada');

// redireciona para outra rota a partir do nome da mesma
Route::get('/rota_referenciada', function() {
    return redirect()->route('rota_nomeada');
});
```

## grupos:

```
/* grupo de rotas que utilizam o mesmo prefixo, resultando nas urls:
/admin/home
/admin/about
/admin/manegement
*/

Route::prefix('admin')->group(function(){
    Route::get('/home', [MainController::class, 'index']);
    Route::get('/about', [MainController::class, 'about']);
    Route::get('/manegement', [MainController::class, 'mostrarValor']);
});
```

## outra forma de criar routes que usam o mesmo controller:

```
// outra maneira de criar rotas que usam o mesmo controller

// Route::get('/user/new', [UserController::class, 'new']);
// Route::get('/user/edit', [UserController::class, 'edit']);
// Route::get('/user/delete', [UserController::class, 'delete']);

Route::controller(UserController::class)->group(function(){
    Route::get('/user/new', 'new');
    Route::get('/user/edit', 'edit');
    Route::get('/user/delete', 'delete');
});
```

## rota padrão para quando uma rota não for encontrada

```
Route::fallback(function(){
	echo 'Página não encontrada';
});
```