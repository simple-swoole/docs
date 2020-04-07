# 安装

需要确保运行环境达到了以下的要求：

* PHP >= 7.2
* Swoole PHP 扩展 >= 4.4
* PDO PHP 扩展 （如需要使用到 MySQL 客户端）
* Redis PHP 扩展 （如需要使用到 Redis 客户端）

## 通过 Composer 创建项目

```bash
composer create-project simple-swoole/skeleton:dev-master
```

## 启动

支持HTTP服务、WebSocket服务和MQTT服务

```bash
php bin/simps.php http:start
php bin/simps.php ws:start
php bin/simps.php mqtt:start
```

服务配置文件在`config/servers.php`中
