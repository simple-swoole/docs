# Redis

同样的使用了由[Swoole Library](https://github.com/swoole/library)提供的Redis连接池

## 安装

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

### 订阅

由于现在的设计是使用一个连接操作之后就立刻还回去，执行订阅操作时可能会出现问题，可以参照如下示例代码

```php
use Simps\DB\Redis as SimpsRedis;
use Redis;

while (1) {
    /** @var Redis $redis */
    $redis = SimpsRedis::getInstance()->getConnection();
    $redis->setOption(Redis::OPT_READ_TIMEOUT, "-1");
    $redis->subscribe(['test'], function ($redis, $chan, $msg){
        echo $chan, " ==> ", $msg, PHP_EOL;
    });

}
```