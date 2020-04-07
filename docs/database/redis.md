# Redis

同样的使用了由[Swoole Library](https://github.com/swoole/library)提供的Redis连接池

## 安装

!> Swoole版本 >= v4.4.17 时不需要单独进行安装。如果单独安装了，请在[启动](/install?id=启动)时加上`-d swoole.enable_library=off`关闭内置的`library`

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