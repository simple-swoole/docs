# 使用

框架未处理服务的所有事件，但提供了对应的埋点，用户需要自行处理

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

此回调类必须是一个单例，只需要在类中`use`一下`Singleton`类。

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

