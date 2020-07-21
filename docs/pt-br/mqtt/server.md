# Servidor MQTT

O MQTT é um protocolo de transmissão de mensagens do modelo de publicação / assinatura para a arquitetura cliente-servidor. 
Sua idéia de design é leve, aberta, simples, padronizada e fácil de implementar. Essas características o tornam uma boa 
escolha para muitos cenários, especialmente para ambientes restritos, como ambientes de comunicação máquina a máquina (M2M) 
e Internet das Coisas (IoT).

Leia o conteúdo relevante do contrato MQTT antes do desenvolvimento: [mqtt-v3.1.1](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html)

Ao usar o Swoole como servidor, configurando a opção [open_mqtt_protocol](https://wiki.swoole.com/#/server/setting?id=open_mqtt_protocol), 
o cabeçalho `MQTT` será analisado quando ativado e o` O evento onReceive` do processo `worker` retornará um pacote completo` MQTT` a cada vez.

Esse framework encapsula operações relacionadas ao MQTT e expõe algumas interfaces. No uso real, ele precisa implementar 
`Simps\Server\Protocol\MqttInterface`. Os usuários devem apenas prestar atenção à implementação da lógica de negócios ao 
usar: como assinar mensagens, publicar mensagens etc.

## Achieve

> O `receiveCallbacks` na configuração abaixo é a implementação correspondente `Simps \Server\Protocol\MqttInterface`
Aqui está um exemplo de `onMqConnect`, primeiro crie um novo arquivo `app/Events/MqttServer.php`

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
            // Se o nome do protocolo estiver incorreto, o servidor poderá desconectar o cliente ou continuar processando a mensagem CONNECT de acordo com algumas outras especificações.
            $server->close($fd);
            return false;
        }

        //Determine se o cliente está conectado, se é necessário desconectar a conexão antiga
        //Determinar se há informações do testamento
        // ...

        // Retornar para confirmar a solicitação de conexão
        $server->send(
            $fd,
            MQTT::getAck(
                [
                    'cmd' => 2, //O valor fixo do CONNACK é 2
                    'code' => 0, //Código de retorno da conexão 0 significa que a conexão foi aceita pelo servidor
                    'session_present' => 0
                ]
            )
        );
    }
}
```

## Configuração

O arquivo de configuração está em `config/servers.php`, adicione um servidor MQTT

```php
use Simps\Server\Protocol\MQTT;

return [
    // ... Omitir outra configuração de serviço
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

## Começar

Execute o comando para iniciar o serviço do Servidor MQTT
```bash
php bin/simps.php mqtt:start
```