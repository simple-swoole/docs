# servers.php

此配置文件用于配置Swoole Server的相关参数。

```php
return [
    'mode' => SWOOLE_PROCESS,
    'http' => [
        'ip' => '0.0.0.0',
        'port' => 9501,
        'sock_type' => SWOOLE_SOCK_TCP,
        'callbacks' => [
        ],
        'settings' => [
            'worker_num' => swoole_cpu_num(),
        ],
    ],
    'ws' => [
        'ip' => '0.0.0.0',
        'port' => 9502,
        'sock_type' => SWOOLE_SOCK_TCP,
        'callbacks' => [
            'open' => [\App\Events\WebSocket::class, 'onOpen'],
            'message' => [\App\Events\WebSocket::class, 'onMessage'],
            'close' => [\App\Events\WebSocket::class, 'onClose'],
        ],
        'settings' => [
            'worker_num' => swoole_cpu_num(),
            'open_websocket_protocol' => true,
        ],
    ],
];
```

## 参数说明

* **mode**

设置运行模式，Swoole提供了两个常量：`SWOOLE_PROCESS`和`SWOOLE_BASE`，关于两种模式的详细介绍请参考[Swoole文档](https://wiki.swoole.com/#/learn?id=server%e7%9a%84%e4%b8%a4%e7%a7%8d%e8%bf%90%e8%a1%8c%e6%a8%a1%e5%bc%8f%e4%bb%8b%e7%bb%8d)

* **http/ws/mqtt**

需要启动的服务，框架默认提供了HTTP和WebSocket Server的配置，点击查看[MQTT Server配置](zh-cn/mqtt/server?id=%e9%85%8d%e7%bd%ae)以及[多端口监听配置](zh-cn/multi-port-listen)

下面将说明服务中具体参数：

1. **ip/port**
    
设置指定监听的 ip 地址和端口

2. **sock_type**

由Swoole提供的 Socket 类型，其他常量和作用请参考[Swoole文档](http://wiki.swoole.com/#/consts?id=socket-%E7%B1%BB%E5%9E%8B)

3. **callbacks**

回调事件，详情请参考[未定义事件](zh-cn/listens?id=未定义事件)

在MQTT的配置当中会多一个`receiveCallbacks`配置项，作用是在onReceive事件中处理对应的MQTT事件，具体配置请参考[对应的文档](zh-cn/mqtt/server?id=配置)

4. **settings**

用于设置一些由Swoole提供的配置参数，具体的可配置参数请参考[Swoole文档](http://wiki.swoole.com/#/server/setting)