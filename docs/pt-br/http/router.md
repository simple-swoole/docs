# Roteador

A rota do servidor HTTP é analisada usando [nikic/fast-route](https://github.com/nikic/FastRoute), e a rota é 
analisada e distribuída para o controlador correspondente enquanto escuta o evento `onRequest`.

## Definição do roteador

Declare as informações de roteamento correspondentes no arquivo `config/routes.php`.
```php
return [
    ['GET', '/', '\App\Controller\IndexController@index'],
    ['POST', '/', '\App\Controller\IndexController@index'],
    ['GET', '/hello[/{name}]', '\App\Controller\IndexController@hello'],
    ['GET', '/favicon.ico', function ($request, $response) {
        $response->end('');
    }],
];
```

## Parâmetros do roteador

O método do controlador correspondente possui dois parâmetros `$request` e `$response` por padrão. Se parâmetros de 
roteamento forem adicionados, eles estarão na terceira matriz de parâmetros.
```php
public function hello($request, $response, $data)
{
    $name = $data['name'] ?? 'Simps';

    $response->end(
        json_encode(
            [
                'method' => $request->server['request_method'],
                'message' => "Hello {$name}.",
            ]
        )
    );
}
```

Por exemplo, a rota padrão `/hello[/{name}]`, desta vez quando você acessa o correspondente `http://0.0.0.0:9501/hello/swoole`, 
você gera

```json
{
    "method": "GET",
    "message": "Olá swoole."
}
```