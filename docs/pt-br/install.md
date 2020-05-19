# Instalação

É necessário garantir que o ambiente operacional atenda aos seguintes requisitos:

* PHP >= 7.1
* extensão do Swoole para PHP  >= 4.4
* extensão PDO（Se você necessitar de usar o Cliente MySQL）
* extensão Redis para PHP （Se você necessitar de usar o Cliente Redis）

## Criar o projeto pelo Composer

```bash
composer create-project simple-swoole/skeleton
```

!> Se você precisar usar os clientes `MySQL` e `Redis`, e a versão `Swoole` for menor que `v4.4.17`, será necessário 
instalar o [Swoole / Library](https://github.com/swoole/library) , ao mesmo tempo, você precisa adicionar 
`-d swoole.enable_library = off` ou modificar o `php.ini` para fechar a `biblioteca` embutida na inicialização
```bash
composer require swoole/library:dev-master
```

## Início

Servidor HTTP, Servidor WebSocket e Servidor MQTT.

```bash
php bin/simps.php http:start
php bin/simps.php ws:start
php bin/simps.php mqtt:start
```

O arquivo de configuração do servidor está em `config/servers.php`.

## Servidor Padrão

Por exemplo, se você necessitar de separar o serviço TCP:

```php
return [
    // Outras partes omitidas.
    'tcp' => [
        'ip' => '0.0.0.0',
        'port' => 9504,
        'sock_type' => SWOOLE_SOCK_TCP,
        'callbacks' => [
        ],
        'settings' => [
        ],
        // Configurando a classe do servidor de inicialização
        'class_name' => \App\Server\TCP::class
    ],
];
```
A execução do comando a seguir iniciará o serviço correspondente.
```shell
php bin/simps.php tcp:start
```