# Uso

## Evento já existe

Os eventos existentes do framework fornecem um ponto de sepultamento. Quando você precisa aumentar os negócios, os 
usuários precisam lidar com isso sozinhos.

Definido no arquivo `config/listeners.php`, suportando vários callbacks em um evento.

O evento `onWorkStart` é definido por padrão para inicializar o conjunto de conexões na inicialização.

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

Ou seja, durante o WorkerStart, o método `workerStart` da classe `\App\Listens\Pool` será chamado de volta.

!> Essa classe de callback deve ser um singleton, basta usar a classe `Simps\Singleton` na classe.
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

## Event indefinido

Eventos não definidos pelo framework, como o `onClose`, `onOpen`, etc., podem ser definidos em `callbacks` no arquivo `config/servers.php`.

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

Indica que o evento `onClose` será chamado de volta ao método estático `onClose` da classe `\App\Events\HTTP`.

!> Deve ser um método estático com os mesmos parâmetros que correspondem aos parâmetros de eventos.