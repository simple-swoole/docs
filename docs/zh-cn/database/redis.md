# Redis

同样的使用了由[Swoole Library](https://github.com/swoole/library)提供的Redis连接池

## 安装

```bash
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
$redis->get('key');
```

## 队列

```bash
composer require easyswoole/queue 3.x
```

```php
use EasySwoole\Queue\Driver\RedisQueue;
use EasySwoole\Queue\Job;
use EasySwoole\Queue\Queue;
use EasySwoole\Redis\Config\RedisConfig;

// 生产
$config = new RedisConfig();
$queue = new Queue(new RedisQueue($config));

$job = new Job();
$job->setJobData("this is my job data time time " . date('Ymd h:i:s'));
var_dump($queue->producer()->push($job));

// 消费
$config = new RedisConfig();
$queue = new Queue(new RedisQueue($config));
$job = $queue->consumer()->pop();
var_dump($job->getJobData());
```