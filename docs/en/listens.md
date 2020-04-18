# Use

## Event already exists

The existing events of the framework provide a burial point. When you need to increase business, users need to deal with it by themselves.

Defined in the `config/listeners.php` file, supporting multiple callbacks in one event.

The `onWorkStart` event is defined by default to initialize the connection pool at startup.

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

That is, during WorkerStart, the `workerStart` method of the `\App\Listens\Pool` class will be called back.

!> This callback class must be a singleton, just use `Simps\Singleton` class in the class.

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

## Undefined event

Events not defined by the framework, such as `onClose`,` onOpen`, etc., can be defined in `callbacks` in the `config/servers.php` file.

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

Indicates that the `onClose` event will be called back to the static method` onClose` of the `\App\Events\HTTP` class.

!> Must be a static method with the same parameters as the corresponding event parameters