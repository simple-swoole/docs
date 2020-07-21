# MQTT服务端

MQTT是一个客户端服务端架构的发布/订阅模式的消息传输协议。它的设计思想是轻巧、开放、简单、规范，易于实现。这些特点使得它对很多场景来说都是很好的选择，特别是对于受限的环境如机器与机器的通信（M2M）以及物联网环境（IoT）。

在开发前请阅读了解MQTT协议的相关内容：[MQTT协议3.1.1中文翻译版](https://mcxiaoke.gitbook.io/mqtt/01-introduction)

使用Swoole作为服务端时，通过设置 [open_mqtt_protocol](https://wiki.swoole.com/#/server/setting?id=open_mqtt_protocol) 选项，启用后会解析 `MQTT` 包头，`worker` 进程的 `onReceive` 事件每次会返回一个完整的 `MQTT` 数据包。

本框架封装了MQTT相关操作，并暴露了一些接口，在实际使用时需要实现`Simps\Server\Protocol\MqttInterface`，用户在使用时应该只需要关注业务逻辑实现：如订阅消息、发布消息等

!> 如有定制开发需求可以联系QQ：85464277。

## 实现

!> 下文配置中的`receiveCallbacks`就是对应的实现`Simps\Server\Protocol\MqttInterface`

这里以`onMqConnect`举例说明一下，首先新建一个`app/Events/MqttServer.php`文件

```php
<?php

declare(strict_types=1);
/**
 * This file is part of Simps.
 *
 * @link     https://simps.io
 * @document https://doc.simps.io
 * @license  https://github.com/simple-swoole/simps/blob/master/LICENSE
 */

namespace App\Events;

use Simps\Server\Protocol\MQTT;
use Simps\Server\Protocol\MqttInterface;

class MqttServer implements MqttInterface
{
    public function onMqConnect($server, int $fd, $fromId, $data)
    {
        if ($data['protocol_name'] != "MQTT") {
            // 如果协议名不正确服务端可以断开客户端的连接，也可以按照某些其它规范继续处理CONNECT报文
            $server->close($fd);
            return false;
        }

        // 判断客户端是否已经连接，如果是需要断开旧的连接
        // 判断是否有遗嘱信息
        // ...

        // 返回确认连接请求
        $server->send(
            $fd,
            MQTT::getAck(
                [
                    'cmd' => 2, // CONNACK固定值为2
                    'code' => 0, // 连接返回码 0表示连接已被服务端接受
                    'session_present' => 0
                ]
            )
        );
    }
}
```

## 配置

配置文件在`config/servers.php`中，增加一个MQTT的服务

```php
use Simps\Server\Protocol\MQTT;

return [
    // ... 省略了其他服务配置
    'mqtt' => [
        'ip' => '0.0.0.0',
        'port' => 9503,
        'callbacks' => [
        ],
        'receiveCallbacks' => [
            MQTT::CONNECT => [\App\Events\MqttServer::class, 'onMqConnect'],
            MQTT::PINGREQ => [\App\Events\MqttServer::class, 'onMqPingreq'],
            MQTT::DISCONNECT => [\App\Events\MqttServer::class, 'onMqDisconnect'],
            MQTT::PUBLISH => [\App\Events\MqttServer::class, 'onMqPublish'],
            MQTT::SUBSCRIBE => [\App\Events\MqttServer::class, 'onMqSubscribe'],
            MQTT::UNSUBSCRIBE => [\App\Events\MqttServer::class, 'onMqUnsubscribe'],
        ],
        'settings' => [
            'worker_num' => 1,
            'open_mqtt_protocol' => true,
        ],
    ],
];
```

## 启动

执行命令即可启动MQTT Server服务

```bash
php bin/simps.php mqtt:start
```