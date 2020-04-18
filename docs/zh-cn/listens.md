# 使用

## 已存在事件

框架已存在的事件提供了埋点，需要增加业务时，用户需要自行处理

定义在`config/listeners.php`文件中，支持一个事件中多个回调

默认定义了`onWorkStart`事件，在启动时初始化连接池

```php
<?php

declare(strict_types=1);

return [
    //Server::onWorkerStart
    'workerStart' => [
        [\App\Listens\Pool::class, 'workerStart'],
    ],
];
```

即在WorkerStart时，会回调`\App\Listens\Pool`类的`workerStart`方法

!> 此回调类必须是一个单例，只需要在类中`use Simps\Singleton`。

```php
<?php

declare(strict_types=1);

namespace App\Listens;

use Simps\Singleton;

class Pool
{
    use Singleton;

    public function workerStart($server, $workerId)
    {
    }
}
```

## 未定义事件

框架未定义的事件，例如`onClose`、`onOpen`等，用户可在`config/servers.php`文件中的`callbacks`中定义

```php
return [
    'mode' => SWOOLE_PROCESS,
    'http' => [
        'ip' => '0.0.0.0',
        'port' => 9501,
        'sock_type' => SWOOLE_SOCK_TCP,
        'callbacks' => [
            "close" => [\App\Events\HTTP::class, 'onClose'],
        ],
        'settings' => [
            'worker_num' => swoole_cpu_num(),
        ],
    ],
];
```

表示`onClose`事件会回调至`\App\Events\HTTP`类的静态方法`onClose`

!> 必须为静态方法，参数和对应事件参数相同