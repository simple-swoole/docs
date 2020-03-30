# Redis

同样的使用了由[Swoole Library](https://github.com/swoole/library)提供的Redis连接池

## 安装

!> 如果使用`skeleton`进行安装的就不用在单独进行安装。

```
composer require simple-swoole/db
```

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
    'size' => 64,
];
```

## 使用

```php
$redis = new \Simps\DB\BaseRedis();
$redis->get("key");
```