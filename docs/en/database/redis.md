# Redis

The same uses the Redis connection pool provided by [Swoole Library](https://github.com/swoole/library)

## Installation

```
composer require simple-swoole/db
```

## Configuration

the configuration file is in `config/redis.php`

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

## Use

```php
$redis = new \Simps\DB\BaseRedis();
$redis->get('key');
```