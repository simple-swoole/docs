# MySQL

使用了由 [Swoole Library](https://github.com/swoole/library) 提供的MySQL连接池。

## 安装

```
composer require simple-swoole/db
```

## 配置

使用PDO进行连接，配置文件在`config/database.php`中

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

## 使用

### Medoo

集成了轻量级的PHP数据库框架 [Medoo](https://medoo.lvtao.net/index.php) ，使用时需要继承`Simps\DB\BaseModel`，所以使用方法和Medoo基本一致，具体请查看[Medoo的文档](https://medoo.lvtao.net/1.2/doc.php)

唯一不同的是事务相关操作，在Medoo中是使用`action( $callback )`方法，而在本框架中也可以使用`action( $callback )`方法，另外也支持以下方法

```php
beginTransaction(); // 开启事务
commit(); // 提交事务
rollBack(); // 回滚事务
```

#### 示例

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

#### 方法

|      方法名      |   返回值类型    |                  备注                   |
| :--------------: | :-------------: | :-------------------------------------: |
| beginTransaction |     `void`      |                开启事务                 |
|      commit      |     `void`      |                提交事务                 |
|     rollBack     |     `void`      |                回滚事务                 |
|      insert      |      `int`      | 插入数据，返回主键 ID，非自增主键返回 0 |
|     execute      |      `int`      |       执行 SQL，返回受影响的行数        |
|      query       |     `array`     |        查询 SQL，返回结果集列表         |
|      fetch       | `array, object` |     查询 SQL，返回结果集的首行数据      |
