# csrf

- Em formulários se faz necessário adicionar `@csrf` que irá adicionar uma camada de proteção encriptada

```
<form action="{{ route('newNoteSubmit') }}" method="post">
	@csrf
</form>
```