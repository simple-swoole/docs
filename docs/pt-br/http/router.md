# Router

The route of the HTTP server is parsed using [nikic/fast-route](https://github.com/nikic/FastRoute), and the route is parsed and distributed to the corresponding controller while listening for the `onRequest` event.

## Router definition

Declare the corresponding routing information in the `config/routes.php` file.

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

## Router parameters

The corresponding controller method has two parameters `$request` and `$response` by default. If routing parameters are added, they are in the third parameter array.

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

For example, the default route `/hello[/{name}]`, this time when you access the corresponding `http://0.0.0.0:9501/hello/swoole`, you will output

```json
{
    "method": "GET",
    "message": "Hello swoole."
}
```