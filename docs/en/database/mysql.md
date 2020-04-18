# MySQL

The MySQL connection pool provided by [Swoole Library](https://github.com/swoole/library) is used, and a lightweight PHP database framework [Medoo](https://medoo.lvtao.net/index.php), you need to inherit `Simps\DB\BaseModel`

## Installation

```
composer require simple-swoole/db
```

## Configuration

Use PDO to connect, the configuration file is in `config/database.php`

```php
<?php

declare(strict_types=1);

return [
    'host' => 'localhost',
    'port' => 3306,
    'database' => 'root',
    'username' => 'root',
    'password' => '',
    'charset' => 'utf8mb4',
    'options' => [
    ],
    'size' => 64 // 连接池size
];
```

## Use

The functions of Medoo are integrated, so the method of use is basically the same as Medoo. For details, please check [Medoo's documentation](https://medoo.lvtao.net/1.2/doc.php)

The only difference is transaction-related operations. In Medoo, the `action( $callback )` method is used. In this framework, the `action( $callback )` method can also be used. In addition, the following methods are also supported

```php
beginTransaction();
commit();
rollBack();
```

### Examples

```php
$this->beginTransaction();

$this->insert("user", [
    "name" => "luffy",
    "gender" => "1"
]);

$this->delete("user", [
    "id" => 2
]);

if ($this->has("user", ["id" => 23]))
{
    $this->rollBack();
} else {
    $this->commit();
}
```