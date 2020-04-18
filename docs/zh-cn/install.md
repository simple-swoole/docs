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

!> 如果你需要使用`MySQL`和`Redis`客户端，并且`Swoole`版本小于`v4.4.17`，则需要单独安装一下[Swoole/Library](https://github.com/swoole/library)，同时也需要在启动时加上`-d swoole.enable_library=off`或者修改`php.ini`关闭内置的`library`

```bash
composer require swoole/library:dev-master
```

## 启动

支持HTTP服务、WebSocket服务和MQTT服务

```bash
php bin/simps.php http:start
php bin/simps.php ws:start
php bin/simps.php mqtt:start
```

服务配置文件在`config/servers.php`中
