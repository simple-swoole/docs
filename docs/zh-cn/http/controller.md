# 控制器

通过控制器来处理对应的`HTTP`请求，需要通过配置文件将路由与控制器方法进行绑定。

## 编写控制器

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