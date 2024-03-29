# Cliente MQTT

O cliente MQTT é implementado por `Swoole\Coroutine\Client` e fornece os seguintes métodos

> A utilização normal requer uma versão alta de Swoole, Swoole versão >= v4.4.19.

## Método

### __construct()

Crie uma instância do cliente MQTT.

```php
Simps\Client\MQTTClient::__construct(array $config)
```

* `array $config`

Matriz de opções do cliente, você pode definir as seguintes opções:

```php
$config = [
    'host' => '127.0.0.1', // IP do servidor MQTT
    'port' => 1883, // porta do servidor MQTT
    'time_out' => 5, // tempo limite para conectar-se ao servidor MQTT, padrão 0,5 segundos
    'username' => 'username', // nome de usuário
    'password' => 'password', // senha
    'client_id' => '', // ID do cliente
    'keepalive' => 10, // padrão 0 segundos, definido como 0 significa desativado
    'debug' => false, // Os pacotes recebidos serão impressos quando for ligado.
];
```

### connect()

Connect Broker

```php
Simps\Client\MQTTClient->connect(bool $clean = true, array $will = [])
```

* `bool $clean`

Limpe a sessão, o padrão é `true`

* `array $will`

Mensagem do testamento, quando o cliente se desconecta, o Broker envia automaticamente a mensagem do testamento para outros clientes.

O conteúdo a ser definido é o seguinte:

```php
$will = [
    'topic' => '', // tema
    'qos' => 1, // Nível de QoS
    'retain' => 0, // reter marca
    'content' => '', // conteúdo
];
```

### publish()

Poste uma mensagem em um tópico.

```php
Simps\Client\MQTTClient->publish($topic, $content, $qos = 0, $dup = 0, $retain = 0)
```

### subscribe()

Inscreva-se em um tópico ou em vários tópicos.

```php
Simps\Client\MQTTClient->subscribe(array $topics)
```

* `array $topics`

`$topics` é uma matriz em que `key` é um tópico e `value` é `QoS`, por exemplo

```php
$topics = [
    // tema => Qos
    'topic1' => 0, 
    'topic2' => 1,
];
```

### unSubscribe()

Cancele a inscrição em um tópico ou em vários tópicos.

```php
Simps\Client\MQTTClient->unSubscribe(array $topics)
```

* `array $topics`

```php
$topics = ['topic1', 'topic2'];
```

### close()

Normalmente desconecte-se do Broker, a mensagem `DISCONNECT (14)` será enviada ao Broker.

```php
Simps\Client\MQTTClient->close()
```

### recv()

Receber mensagem.

```php
Simps\Client\MQTTClient->recv()
```
O valor de retorno pode ser `bool`, `array`, `string`

> Se for um array, você precisará processar a lógica de acordo com o parâmetro correspondente `cmd`

### sendBuffer()

Enviar mensagem.

```php
Simps\Client\MQTTClient->sendBuffer(array $data, $response = true)
```

* `array $data`

`$data` é o dado a ser enviado e deve conter informações como `cmd`.

* `bool $response`

Se um recibo é necessário, se for `true`, `recv()` será chamado uma vez.

### ping()

Enviar pacote de pulsação.

```php
Simps\Client\MQTTClient->ping()
```

### getMsgId()

Obtenha o número do ID da mensagem atual.

```php
Simps\Client\MQTTClient->getMsgId()
```

## Exemplo de uso

### Publicar

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

### Se inscrever

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

### Cancelar subscrição

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