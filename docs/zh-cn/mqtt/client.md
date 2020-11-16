# MQTT客户端

!> 将于框架`v1.1`版本移除，请使用单独的扩展包 [simps/mqtt](https://github.com/simps/mqtt) ，包含协议解析和协程客户端。

MQTT客户端由`Swoole\Coroutine\Client`实现，提供了以下方法

!> 正常使用需要使用高版本 Swoole，Swoole 版本 >= v4.4.19

## 方法

### __construct()

创建一个MQTT客户端实例。

```php
Simps\Client\MQTTClient::__construct(array $config, array $swConfig = [])
```

* 参数`array $config`

客户端选项数组，可以设置以下选项：

```php
$config = [
    'host' => '127.0.0.1', // MQTT服务端IP
    'port' => 1883, // MQTT服务端端口
    'time_out' => 5, // 连接MQTT服务端超时时间，默认0.5秒
    'username' => 'username', // 用户名
    'password' => 'password', // 密码
    'client_id' => '', // 客户端id
    'keepalive' => 10, // 默认0秒，设置成0代表禁用
    'debug' => false, // 开启后将打印收到的数据包
];
```

* 参数`array $swConfig`

用于设置`Swoole\Coroutine\Client`参数，请参考Swoole文档[set()](https://wiki.swoole.com/#/coroutine_client/client?id=set)

### connect()

连接Broker

```php
Simps\Client\MQTTClient->connect(bool $clean = true, array $will = [])
```

* 参数`bool $clean`

清理会话，默认为`true`

!> 具体描述请参考[清理会话 Clean Session](https://mcxiaoke.gitbook.io/mqtt/03-controlpackets/0301-connect#qing-li-hui-hua-clean-session)

* 参数`array $will`

遗嘱消息，当客户端断线后Broker会自动发送遗嘱消息给其它客户端

需要设置的内容如下

```php
$will = [
    'topic' => '', // 主题
    'qos' => 1, // QoS等级
    'retain' => 0, // retain标记
    'content' => '', // content
];
```

### publish()

向某个主题发布一条消息

```php
Simps\Client\MQTTClient->publish($topic, $content, $qos = 0, $dup = 0, $retain = 0)
```

* 参数`$topic` 主题
* 参数`$content` 内容
* 参数`$qos` QoS等级，默认0
* 参数`$dup` 重发标志，默认0
* 参数`$retain` retain标记，默认0

### subscribe()

订阅一个主题或者多个主题

```php
Simps\Client\MQTTClient->subscribe(array $topics)
```

* 参数`array $topics`

`$topics`的`key`是主题，值为`QoS`的数组，例如

```php
$topics = [
    // 主题 => Qos
    'topic1' => 0, 
    'topic2' => 1,
];
```

### unSubscribe()

取消订阅一个主题或者多个主题

```php
Simps\Client\MQTTClient->unSubscribe(array $topics)
```

* 参数`array $topics`

`$topics`的`key`是主题，值为`QoS`的数组，例如

```php
$topics = ['topic1', 'topic2'];
```

### close()

正常断开与Broker的连接，`DISCONNECT(14)`报文会被发送到Broker

```php
Simps\Client\MQTTClient->close()
```

### recv()

接收消息

```php
Simps\Client\MQTTClient->recv()
```

返回值可能为`bool`、`array`、`string`

!> 如果是数组的话就需要按照对应的`cmd`参数处理逻辑

### sendBuffer()

发送消息

```php
Simps\Client\MQTTClient->sendBuffer(array $data, $response = true)
```

* 参数`array $data`

`$data`是需要发送的数据，必须包含`cmd`等信息

* 参数`bool $response`

是否需要回执，如果为`true`，会调用一次`recv()`

### ping()

发送心跳包

```php
Simps\Client\MQTTClient->ping()
```

### getMsgId()

获取当前消息id条数

```php
Simps\Client\MQTTClient->getMsgId()
```

## 使用示例

### 发布

```php
use Simps\Client\MQTTClient;

$config = [
    'host' => '127.0.0.1',
    'port' => 9503,
    'time_out' => 5,
    'username' => 'username',
    'password' => 'password',
    'client_id' => 'd812edc1-18da-2085-0edf-a4a588c296da',
];

Co\run(function () use ($config) {
    $client = new MQTTClient($config);
    while (! $client->connect()) {
        \Swoole\Coroutine::sleep(3);
        $client->connect();
    }
    $response = $client->publish('simpsmqtt/+/get', '123');
    if ($response) {
        $client->close();
    }
});
```

### 订阅

```php
use Simps\Client\MQTTClient;

$config = [
    'host' => '127.0.0.1',
    'port' => 9503,
    'time_out' => 5,
    'username' => 'username',
    'password' => 'password',
    'client_id' => 'd812edc1-18da-2085-0edf-a4a588c296d1',
    'keepalive' => 10,
];

Co\run(function () use ($config) {
    $client = new MQTTClient($config);
    $will = [
        'topic' => 'simpsmqtt/username/update',
        'qos' => 1,
        'retain' => 0,
        'content' => "123",
    ];
    while (! $client->connect(true, $will)) {
        \Swoole\Coroutine::sleep(3);
        $client->connect(true, $will);
    }
    $topics['simpsmqtt/username/get'] = 1;
    $topics['simpsmqtt/username/update'] = 1;
    $timeSincePing = time();
    $client->subscribe($topics);
    while (true) {
        $buffer = $client->recv();
        var_dump($buffer);
        if ($buffer && $buffer !== true) {
            $timeSincePing = time();
        }
        if (isset($config['keepalive']) && $timeSincePing < (time() - $config['keepalive'])) {
            $buffer = $client->ping();
            if ($buffer) {
                echo '发送心跳包成功' . PHP_EOL;
                $timeSincePing = time();
            } else {
                $client->close();
                break;
            }
        }
    }
});
```

### 取消订阅

```php
use Simps\Client\MQTTClient;

$config = [
    'host' => '127.0.0.1',
    'port' => 9503,
    'time_out' => 5,
    'username' => 'username',
    'password' => 'password',
    'client_id' => 'd812edc1-18da-2085-0edf-a4a588c296da',
];

Co\run(function () use ($config) {
    $client = new MQTTClient($config);
    while (! $client->connect()) {
        \Swoole\Coroutine::sleep(3);
        $client->connect();
    }
    $topics = ['simpsmqtt/username/get'];
    $client->unSubscribe($topics);
    $buffer = $client->recv();
    var_dump($buffer);
    $client->close();
});
```