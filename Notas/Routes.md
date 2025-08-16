# Routes

- Declaração de uma rota:

```
routes/web.php

`Route::get/post('/enderecoDaPagina', [MainController::class, 'metodoDoController'])->name('nomeDaRota');`
```

- Exemplo de uso com o blade na view

```
resources/view/home.blade.php

<a href="{{ route('logout') }}" class="btn btn-outline-secondary px-3">
	Logout<i class="fa-solid fa-arrow-right-from-bracket ms-2"></i>
</a>
```

