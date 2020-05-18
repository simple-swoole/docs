# MultiPort Listen

The `Server` can listen to multiple ports, and each port can be set to handle different protocols, such as port 80 for HTTP protocol and port 9507 for TCP protocol. See [Swoole Document](https://wiki.swoole.com/#/server/port?id=%e5%a4%9a%e7%ab%af%e5%8f%a3%e7%9b%91%e5%90%ac) for details.

## Configuration

The main service and required subservices need to be added to the configuration file `config/servers.php`, such as.

```php
// ...省略了其他服务

// 主服务
'main' => [
    'ip' => '0.0.0.0',
    'port' => 9501,
    'mode' => SWOOLE_BASE, // 不填默认为SWOOLE_PROCESS
    'sock_type' => SWOOLE_SOCK_TCP,
    'class_name' =>  \Swoole\Http\Server::class,
    'callbacks' => [
        "request" => [\App\Events\Http::class, 'onRequest'],
    ],
    'settings' => [
        'worker_num' => swoole_cpu_num(),
    ],
    // 子服务 可以多个
    'sub' => [
        [
            'ip' => '0.0.0.0',
            'port' => 9502,
            'sock_type' => SWOOLE_SOCK_TCP,
            'callbacks' => [
                "receive" => [\App\Events\Tcp::class, 'onReceive'],
            ],
            'settings' => [
                'open_http_protocol' => false, // 设置这个端口关闭HTTP协议功能
            ]
        ],
    ]
],
```

!> The above is a demonstration of using port 9501 for the HTTP protocol and port 9502 for the TCP protocol.

## Start

```shell
php bin/simps.php main:start
```
