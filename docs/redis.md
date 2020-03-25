# Redis

同样的使用了由[Swoole Library](https://github.com/swoole/library)提供的Redis连接池

## 配置

配置文件在`config/redis.php`中

```php
<?php

declare(strict_types=1);

return [
    'host' => 'localhost',
    'port' => 6379,
    'auth' => '',
    'db_index' => 0,
    'time_out' => 1,
];
```

## 使用

```php
$redis = new \Simps\DB\BaseRedis();
$redis->get("key");
```