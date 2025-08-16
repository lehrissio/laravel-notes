
# Diferenças entre Yield e Include no Laravel

|Aspecto|`@yield`|`@include`|
|---|---|---|
|**Propósito**|Define seções que podem ser preenchidas por views filhas|Inclui uma view parcial dentro de outra|
|**Uso**|Template inheritance (herança)|Composição de views|
|**Flexibilidade**|Conteúdo dinâmico definido pelas views filhas|Conteúdo estático ou com dados específicos|
|**Escopo**|Trabalha com layouts pai/filho|Inclui views independentes|

## @yield - Template Inheritance

### Conceito

O `@yield` é usado em layouts para criar "espaços reservados" que serão preenchidos pelas views filhas.

### Sintaxe

```blade
@yield('nome_da_secao', 'conteudo_padrao_opcional')
```

### Exemplo

**Layout (resources/views/layouts/app.blade.php):**

```blade
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title', 'Minha Aplicação')</title>
    @yield('styles')
</head>
<body>
    <header>
        <h1>Meu Site</h1>
    </header>
    
    <main>
        @yield('content')
    </main>
    
    <footer>
        @yield('footer', '<p>&copy; 2024 Todos os direitos reservados</p>')
    </footer>
    
    @yield('scripts')
</body>
</html>
```

**View Filha (resources/views/home.blade.php):**

```blade
@extends('layouts.app')

@section('title', 'Página Inicial')

@section('styles')
    <link rel="stylesheet" href="/css/home.css">
@endsection

@section('content')
    <h2>Bem-vindo!</h2>
    <p>Esta é a página inicial do site.</p>
@endsection

@section('scripts')
    <script src="/js/home.js"></script>
@endsection
```

## @include - Composição de Views

### Conceito

O `@include` inclui uma view parcial dentro de outra, permitindo reutilização de componentes.

### Sintaxe

```blade
@include('nome_da_view', ['variavel' => 'valor'])
```

### Exemplo Prático

**View Principal (resources/views/dashboard.blade.php):**

```blade
@extends('layouts.app')

@section('content')
    <h1>Dashboard</h1>
    
    @include('partials.user-info', ['user' => $user])
    
    <div class="stats">
        @include('partials.stats-card', [
            'title' => 'Vendas',
            'value' => $sales,
            'icon' => 'dollar-sign'
        ])
        
        @include('partials.stats-card', [
            'title' => 'Usuários',
            'value' => $users,
            'icon' => 'users'
        ])
    </div>
    
    @include('partials.recent-activities')
@endsection
```

**Partial (resources/views/partials/user-info.blade.php):**

```blade
<div class="user-info">
    <img src="{{ $user->avatar }}" alt="Avatar">
    <div>
        <h3>{{ $user->name }}</h3>
        <p>{{ $user->email }}</p>
        <span class="role">{{ $user->role }}</span>
    </div>
</div>
```

**Partial (resources/views/partials/stats-card.blade.php):**

```blade
<div class="stats-card">
    <div class="icon">
        <i class="fa fa-{{ $icon }}"></i>
    </div>
    <div class="content">
        <h4>{{ $title }}</h4>
        <p class="value">{{ $value }}</p>
    </div>
</div>
```

## Principais Diferenças

### 1. **Estrutura e Organização**

- **@yield**: Cria uma estrutura hierárquica (pai → filho)
- **@include**: Permite composição horizontal (componentes lado a lado)

### 2. **Flexibilidade de Conteúdo**

- **@yield**: Cada view filha define seu próprio conteúdo
- **@include**: O conteúdo é fixo, mas pode receber dados

### 3. **Reutilização**

- **@yield**: Reutiliza a estrutura do layout
- **@include**: Reutiliza componentes específicos

### 4. **Escopo de Variáveis**

- **@yield**: Views filhas têm acesso às variáveis do controller
- **@include**: Pode receber variáveis específicas ou herdar do escopo pai

## Quando Usar Cada Um?

### Use @yield quando:

- Criar layouts base para múltiplas páginas
- Definir estruturas que variam entre páginas
- Implementar template inheritance
- Precisar de conteúdo completamente dinâmico

### Use @include quando:

- Reutilizar componentes específicos
- Dividir views complexas em partes menores
- Criar elementos que aparecem em múltiplas páginas
- Manter código organizado e modular

## Exemplo Combinado

```blade
{{-- Layout principal --}}
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    @include('partials.header')
    
    <main>
        @yield('content')
    </main>
    
    @include('partials.footer')
</body>
</html>
```

Esta abordagem combina o melhor dos dois mundos: estrutura flexível com `@yield` e componentes reutilizáveis com `@include`.