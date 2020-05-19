# Redis

O mesmo usa o pool de conexões Redis fornecido pelo [Swoole Library](https://github.com/swoole/library)

## Instalação

```
composer require simple-swoole/db
```

## Configuração

o arquivo de configuração está em`config/redis.php`

```php
<?php

declare(strict_types=1);

return [
    'host' => 'localhost',
    'port' => 6379,
    'auth' => '',
    'db_index' => 0,
    'time_out' => 1,
    'size' => 64,
];
```

## Uso

```php
$redis = new \Simps\DB\BaseRedis();
$redis->get("key");
```