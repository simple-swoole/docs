# MySQL

O conjunto de conexões MySQL fornecido pelo [Swoole Library](https://github.com/swoole/library).

## Instalação

```
composer require simple-swoole/db
```

## Configuração

Use PDO para conectar ao banco, o arquivo de configuração fica em `config/database.php`

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
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    ],
    'size' => 64 // Tamanho do conjunto de conexões
];
```

## Uso

### Medoo

Framework PHP leve e integrado a banco de dados [Medoo] (https://medoo.lvtao.net/index.php), precisa herdar `Simps\DB\BaseModel`, 
para que o uso de métodos e Medoo sejam basicamente os mesmos, consulte [Documentação do Medoo] (https://medoo.lvtao.net/1.2/doc.php).

A única diferença são as operações relacionadas à transação. No Medoo, o método `action ($ callback)` é usado. Nesse
framework, o método `action ($ callback)` também pode ser usado. Além disso, os seguintes métodos também são suportados:

```php
beginTransaction();
commit();
rollBack();
```

#### Exemplos

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

### Banco de Dados

#### Método

|  Nome do método  | Tipo de valor de retorno |                                  Notas                                   |
| :--------------: | :----------------------: | :----------------------------------------------------------------------: |
| beginTransaction |         `void`           |                         Começar uma beginTransaction                     |
|      commit      |         `void`           |                        commit uma beginTransaction                       |
|     rollBack     |         `void`           |                       rollBack uma beginTransaction                      |
|      insert      |          `int`           | Inserir dados, retornar ID da chave primária, chave primária não auto-adicionada retorna 0|
|     execute      |          `int`           |            Execute SQL e retorne o número de linhas afetadas        |
|      query       |         `array`          |               Query SQL e retorne uma lista de conjuntos de resultados.                |
|      fetch       |     `array, object`      |       Query SQL e retorne a primeira linha de dados no conjunto de resultados      |
