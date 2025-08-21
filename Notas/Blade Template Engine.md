# Blade Template Engine

- (views)
- if else:
```
{{-- if else --}}
@if($value < 100)
    <p>Primeiro</p>
@elseif($value < 200)
    <p>Segundo</p>
@else
    <p>Outro</p>
@endif
```

- switch:
```
{{-- switch --}}
@switch($value)
    @case (100)
        <p>Valor 100</p>
        @break
    @case (200)
        <p>Valor 200</p>
        @break
    @case (300)
        <p>Valor 300</p>
        @break
    @default
        <p>Outro</p>
@endswitch
```

- empty:
```
{{-- empty --}}
@empty($value)
    <p>Está vazia</p>
@else
    <p>Não está vazia</p>
@endempty
```

- isset:
```
{{-- isset --}}
@isset($value)
    <p>Existe</p>
@else
    <p>Não existe</p>
@endisset
```

- unless:
```
{{-- unless --}}
@unless($value == 100)
    <p>Ok</p>
@endunless
```

- for:
```
{{-- for --}}
@for($i = 0; $i < 5; $i++)
    <h1>{{ $i }}</h1>
@endfor
```

- foreach
```
{{-- foreach --}}
@foreach($cities AS $city)
    <h1>{{ $city }}</h1>
@endforeach
```

- forelse:
```
{{-- forelse --}}
@forelse($names AS $name)
    <p>{{ $name }}</p>
@empty
    <p>Names esta vazio</p>
@endforelse
```

- while:
```
{{-- while --}}
@while($index < 10)
    <p>index: {{ $index }}</p>

    @php
        $index++;
    @endphp
@endwhile

{{-- usando o continue e o break --}}
@for($i = 0; $i < 10; $i++)
    {{-- continue --}}
    @if($i == 5)
        @continue
    @endif    

    <p>index: {{ $i }}</p>
  
    {{-- break --}}
    @if($i == 7)
        @break
    @endif
@endfor
```

- loop variable:
```
{{-- loop variable --}}
@foreach ($cities as $city)
    <h1>{{ $city }}</h1>
    <h1>{{ $loop->index }}</h1>

    @if ($loop->first)
        <h3>Primeira cidade</h3>
    @endif

    @if ($loop->last)
        <h3>Última cidade</h3>
    @endif
@endforeach
```

- executar php dentro de uma view:
```
{{-- executar PHP dentro de uma view --}}
@php
    $valor = 100;
    $valor1 = '<p>Texto</p>';
    $nome = 'leticia'
@endphp

<h3>{{ $valor }}</h3>
{{-- para o browser executar o html corretamente --}}
<h3>{!! $valor1 !!}</h3>
<h3> $nome tem <span class="text-info">{{ strlen($nome) }}</span> carcteres</h3>
```

- production (executa apenas se no arquivo .env a constante APP_ENV estiver configurada como produção)
```
@production
    <p>Estou ema mbiente de produtção</p>    
@else
    <p>Não estou em ambiente de produção</p>
@endproduction
```

- env (checa o nome da constante APP_ENV no arquivo .env)
```
@env(['local', 'development'])
    <p>Estou no ambiente {{ env('APP_ENV') }}</p>
@endenv
```

- error (geralmente usada para formulário que contém validações)
```
@error('name')
    <div class="alert alert-danger">{{ $message }}</div>
@enderror
```

- section:
```
// rotas

Route::get('/setSession', [MainController::class, 'setSession']);
Route::get('/clearSession', [MainController::class, 'clearSession']);
```

```
// controller

public function setSession(Request $request): View
{
	$request->session()->put('exercises', $exercises);
	// ou
	session(['name' => 'leticia']);
	return view('home');
}

public function clearSession(): View
{
	session()->forget('name');
	return view('home');
}
```

```
{{-- view --}}
@session('name')
	<p>A sessão tem o valor {{ session('name' }}</p>
@endsession
```