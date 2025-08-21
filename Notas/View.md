# view

`php artisan make:view home`

- controller:

```
class MainController extends Controller
{
    public function showView(): View
    {
        return view('home');
    }
}
```

- route:

```
Route::get('/', [MainController::class, 'showView']);
```

## como passar dados para a view

- view:

```
<p>{{ $name }} e {{ $phone }}</p>
```

- controller:

```
class MainController extends Controller

{
    public function showView(): View
    {
		// metodo 1
        $data = [
            'name' => 'Leticia',
            'phone' => '123456789'
        ];

        // metodo 2
        return view('home', [
            'name' => 'Leticia',
            'phone' => '123456789'
        ]);

        // metodo 3
        return view('home')
                ->with('name', 'Leticia')
                ->with('phone', '123456789');

        // metodo 4
        $name = 'Leticia';
        $phone = '123456789';

        return view('home', compact('name', 'phone'));
    }
}
```

- route:

```
Route::get('/', [MainController::class, 'showView']);
```