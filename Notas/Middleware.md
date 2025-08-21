# Middleware

- Funciona como um intermediário antes de uma requisição chegar ao controlador
- Ex.: para a autenticação no caso de páginas de login.
- É um código que é executado em determinadas rotas, que permite fazer de forma centralizada a pergunta "O usuário está logado?", caso não, irá redirecionar para a página de login.
- Pode ser usado para bloquear o acesso a telas de logout quando o usuário não estiver logado, por exemplo.
- Comando para criar middleware:
	`php artisan make:middleware CheckIsLogged`

- Exemplo de middleware:

```
App/Http/Middleware/CheckIsLogged.php

class CheckIsLogged
{
    public function handle(Request $request, Closure $next): Response
    {
        // checa se o usuário está logado
        if(!session('user')){
            return redirect('/login');
        }
        return $next($request);
    }
}
```

- Exemplo de rotas:

```
routes/web.php

// executa um middleware antes da execução de um grupo de rotas
Route::middleware([CheckIsLogged::class, EndMiddleware::class])->group(function(){
    Route::get('/logout', [AuthController::class, 'logout']);
    Route::get('/', [MainController::class, 'index'])->withoutMiddleware([EndMiddleware::class]);
    Route::get('/newNote', [MainController::class, 'newNote'])->middleware([StartMiddleware::class]);
});

// para uma rota apenas
Route::get('/logout2', [AuthController::class, 'logout'])->middleware([CheckIsLogged::class]);

```

- Middleware que é executado antes do controller

```
class StartMiddleware
{
    public function handle(Request $request, Closure $next): Response
    {
        // vai ser executado antes do controller
        echo '<p>Start Middleware</p>';
        return $next($request);
    }
}
```

- Middleware que é executado depois do controller

```
class EndMiddleware
{
    public function handle(Request $request, Closure $next): Response
    {
        $response = $next($request);
        // vai ser executado antes do controller
        echo '<p>End Middleware</p>';
        return $response;
    }
}
```

# Middleware global

```
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware) {
    
        // adicionar a todas as rotas
        $middleware->prepend([
            StarMiddleware::class
        ]);
  
        // adicionar no final de todas as respostas de todas as rotas
        $middleware->append([
            EndMiddleware::class
        ]);

        // criar grupo de middleware
        $middleware->prependToGroup('executar_antes', [
            StarMiddleware::class
        ]);
    })

    ->withExceptions(function (Exceptions $exceptions) {
        //
    })->create();
```

- Rota para adicionar um grupo de middleware global para um grupo de rotas

```
Route::middleware(['executar_antes'])->group(function(){
    Route::get('/', [MainController::class, 'index'])->name('index');
});
```