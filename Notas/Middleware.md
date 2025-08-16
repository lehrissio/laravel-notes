# Middleware

- Funciona como um intermediário para a autenticação no caso de páginas de login.
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
Route::middleware([CheckIsLogged::class])->group(function(){
    Route::get('/logout', [AuthController::class, 'logout']);
    Route::get('/', [MainController::class, 'index']);
    Route::get('/newNote', [MainController::class, 'newNote']);
});
```