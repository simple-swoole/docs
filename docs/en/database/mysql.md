# MySQL

The MySQL connection pool provided by [Swoole Library](https://github.com/swoole/library) is used.

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
    'unixSocket' => null,
    'options' => [
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    ],
    'size' => 64 // 连接池size
];
```

## Use

### Medoo

Integrated lightweight PHP database framework [Medoo](https://medoo.lvtao.net/index.php), need to inherit `Simps\DB\BaseModel`, so the use of methods and Medoo basically the same, please see [Medoo's documentation](https://medoo.lvtao.net/1.2/doc.php).

The only difference is transaction-related operations. In Medoo, the `action( $callback )` method is used. In this framework, the `action( $callback )` method can also be used. In addition, the following methods are also supported

```php
beginTransaction();
commit();
rollBack();
```

#### Examples

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

### DB

#### Method

|   Method name    | Return Value Type |                                  Notes                                   |
| :--------------: | :---------------: | :----------------------------------------------------------------------: |
| beginTransaction |      `void`       |                         Start a beginTransaction                         |
|      commit      |      `void`       |                        commit a beginTransaction                         |
|     rollBack     |      `void`       |                       rollBack a beginTransaction                        |
|      insert      |       `int`       | Insert data, return primary key ID, non-self-added primary key returns 0 |
|     execute      |       `int`       |            Execute SQL and return the number of affected rows            |
|      query       |      `array`      |               Query SQL and return a list of result sets.                |
|      fetch       |  `array, object`  |       Query SQL and return the first row of data in the result set       |
