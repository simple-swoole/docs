# Router

The route of the HTTP server is parsed using [nikic/fast-route](https://github.com/nikic/FastRoute), and the route is parsed and distributed to the corresponding controller while listening for the `onRequest` event.

## Router definition

Declare the corresponding routing information in the `config/routes.php` file.

```php
return [
    ['GET', '/', '\App\Controller\IndexController@index'],
    ['POST', '/', '\App\Controller\IndexController@index'],
    ['GET', '/test/{id:\d+}', '\App\Controller\IndexController@test'],
];
```

## Router parameters

The corresponding controller method has two parameters `$request` and `$response` by default. If routing parameters are added, they are in the third parameter array.

For example, the default route `/test/{id:\d+}`, this time when you access the corresponding `http://0.0.0.0:9501/test/1`, you will output

```json
{
    "id": "1"
}
```