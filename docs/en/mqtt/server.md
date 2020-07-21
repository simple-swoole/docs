# MQTT Server

MQTT is a publish/subscribe model message transmission protocol for client-server architecture. Its design idea is light, open, simple, standardized and easy to implement. These characteristics make it a good choice for many scenarios, especially for restricted environments such as machine-to-machine communication (M2M) and Internet of Things (IoT) environments.

Please read the relevant content of MQTT agreement before development:[mqtt-v3.1.1](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html)

When using Swoole as the server, by setting the [open_mqtt_protocol](https://wiki.swoole.com/#/server/setting?id=open_mqtt_protocol) option, the `MQTT` header will be parsed when enabled, and the` onReceive` event of the `worker` process will return a complete` MQTT` packet each time.

This framework encapsulates MQTT related operations and exposes some interfaces. In actual use, it needs to implement `Simps\Server\Protocol\MqttInterface`. Users should only need to pay attention to business logic implementation when using: such as subscribing to messages, publishing messages, etc.

## Achieve

!> The `receiveCallbacks` in the configuration below is the corresponding implementation`Simps\Server\Protocol\MqttInterface`

Here is an example of `onMqConnect`, first create a new`app/Events/MqttServer.php`file

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

## Configuration

The configuration file is in `config / servers.php`, add a MQTT Server

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

## Start

Execute the command to start the MQTT Server service

```bash
php bin/simps.php mqtt:start
```