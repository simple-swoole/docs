# 多端口监听

`Server` 可以监听多个端口，每个端口都可以设置不同的协议处理方式，例如 80 端口处理 http 协议，9507 端口处理 TCP 协议。详细介绍请查看 [Swoole文档](https://wiki.swoole.com/#/server/port?id=%e5%a4%9a%e7%ab%af%e5%8f%a3%e7%9b%91%e5%90%ac)。

## 配置

需要在配置文件`config/servers.php`中添加主服务和需要的子服务，如：

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
        'request' => [\App\Events\Http::class, 'onRequest'],
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
                'receive' => [\App\Events\Tcp::class, 'onReceive'],
            ],
            'settings' => [
                'open_http_protocol' => false, // 设置这个端口关闭HTTP协议功能
            ]
        ],
    ]
],
```

!> 以上就是使用 9501 端口处理 http 协议，9502 端口处理 TCP 协议的演示

## 启动

```shell
php bin/simps.php main:start
```
