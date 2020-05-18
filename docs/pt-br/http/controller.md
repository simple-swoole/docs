# Controlador

Para manipular a solicitação HTTP correspondente por meio do controlador, é necessário vincular a rota ao método do 
controlador por meio do arquivo de configuração.

## Controlador de escrita

```php
<?php

declare(strict_types=1);

namespace App\Controller;

class IndexController
{
    public function index($request, $response)
    {
        $response->end("olá swoole");
    }

    public function test($request, $response, $data)
    {
        $response->end(json_encode($data));
    }
}
```