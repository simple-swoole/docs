# Controller

To handle the corresponding HTTP request through the controller, you need to bind the route to the controller method through the configuration file.

## Write controller

```php
<?php

declare(strict_types=1);

namespace App\Controller;

class IndexController
{
    public function index($request, $response)
    {
        $response->end("hello swoole");
    }

    public function test($request, $response, $data)
    {
        $response->end(json_encode($data));
    }
}
```