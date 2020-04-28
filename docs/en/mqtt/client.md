# MQTT客户端

The MQTT client is implemented by `Swoole\Coroutine\Client` and provides the following methods

## Method

### Client::__construct()

Create an MQTT client instance.

```php
public function __construct(array $config)
```

* `array $config`

Client option array, you can set the following options:

```php
$config = [
    'host' => '127.0.0.1', // MQTT服务端IP
    'port' => 1883, // MQTT服务端端口
    'time_out' => 5, // 连接MQTT服务端超时时间，默认0.5秒
    'username' => 'username', // 用户名
    'password' => 'password', // 密码
    'client_id' => '', // 客户端id
    'keepalive' => 10, // 默认0秒，设置成0代表禁用
];
```

### Client::connect()

Connect Broker

```php
public function connect(bool $clean = true, array $will = [])
```

* `bool $clean`

Clean up the session, the default is `true`

* `array $will`

Testament message, when the client disconnects, Broker will automatically send the testament message to other clients.

The content to be set is as follows:

```php
$will = [
    'topic' => '', // 主题
    'qos' => 1, // QoS等级
    'retain' => 0, // retain标记
    'content' => "", // content
];
```

### Client::publish()

Post a message to a topic.

```php
public function publish($topic, $content, $qos = 0, $dup = 0, $retain = 0)
```

### Client::subscribe()

Subscribe to one topic or multiple topics.

```php
public function subscribe(array $topics)
```

* `array $topics`

`$topics` is an array where `key` is a topic and `value` is `QoS`, for example

```php
$topics = [
    // 主题 => Qos
    'topic1' => 0, 
    'topic2' => 1,
];
```

### Client::unSubscribe()

Unsubscribe from one topic or multiple topics.

```php
public function unSubscribe(array $topics)
```

!> The parameters are the same as above.

### Client::close()

Normally disconnect with Broker, `DISCONNECT(14)` message will be sent to Broker.

```php
public function close()
```

### Client::recv()

Receive message.

```
public function recv()
```

The return value may be `bool`,` array`, `string`

!> If it is an array, you need to process logic according to the corresponding `cmd` parameter

### Client::ping()

Send heartbeat packet.

```php
public function ping()
```

### Client::getMsgId()

Get the number of current message id.

```php
public function getMsgId()
```

## Usage example

### Publish

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

### Subscribe

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

### UnSubscribe

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