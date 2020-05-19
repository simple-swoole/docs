# MultiPort Listen

O `Server`  pode ouvir várias portas, e cada porta pode ser configurada para lidar com protocolos diferentes, como a 
porta 80 para o protocolo HTTP e a porta 9507 para o protocolo TCP. Consulte a 
[Documentação do Swoole](https://wiki.swoole.com/#/server/port?id=%e5%a4%9a%e7%ab%af%e5%8f%a3%e7%9b%91%e5%90%ac) para 
obter mais detalhes.

## Configuração

O serviço principal e os subserviços necessários precisam ser adicionados ao arquivo de configuração `config/servers.php`,
como:

```php
// ...Outros serviços omitidos

// Serviço principal
'main' => [
    'ip' => '0.0.0.0',
    'port' => 9501,
    'mode' => SWOOLE_BASE, // O padrão é SWOOLE_PROCESS
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

!> A demonstração acima exemplifica o uso da porta 9501 para o protocolo HTTP e da porta 9502 para o protocolo TCP.

## Começar

```shell
php bin/simps.php main:start
```
