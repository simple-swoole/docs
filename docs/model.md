# 数据库

使用了由[Swoole Library](https://github.com/swoole/library)提供的数据库连接池，使用时需要继承`Simps\DB\BaseModel`

## 配置

支持MySQLi和PDO两种连接方式，配置文件在`config/database.php`中

```php
<?php

declare(strict_types=1);

return [
    'drive' => 'mysqli',
    'host' => 'localhost',
    'port' => 3306,
    'database' => 'root',
    'username' => 'root',
    'password' => '',
    'charset' => 'utf8mb4',
];
```

## 编写

```php
<?php

declare(strict_types=1);

namespace App\Model;

use Simps\DB\BaseModel;

class Model extends BaseModel
{
    public function lists()
    {
        $res1 = $this->query('select * from user');
        return $res1->fetch_all(MYSQLI_ASSOC);
    }
}
```