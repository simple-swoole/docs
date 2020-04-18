# Installation

Need to ensure that the operating environment meets the following requirements:

* PHP >= 7.2
* Swoole PHP extension >= 4.4
* PDO PHP extension （If you need to use MySQL Client）
* Redis PHP extension （If you need to use Redis Client）

## Create project via Composer

```bash
composer create-project simple-swoole/skeleton:dev-master
```

!> If you need to use `MySQL` and` Redis` clients, and `Swoole` version is less than` v4.4.17`, you need to install [Swoole/Library](https://github.com/swoole/library), At the same time, you need to add `-d swoole.enable_library = off` or modify `php.ini` to close the built-in `library` at startup

```bash
composer require swoole/library:dev-master
```

## Start

Support HTTP Server, WebSocket Server and MQTT Server.

```bash
php bin/simps.php http:start
php bin/simps.php ws:start
php bin/simps.php mqtt:start
```

The Server configuration file is in `config/servers.php`.
