# 数据库操作

> 缺少了数据库相关的操作，后端开发将缺少了意义。

## 数据库选择

市面上有很多优秀的数据库，它们的存在总是有意义的，没有绝对好的系统，只有适合自己项目的系统。

LyApi 2.X 采用了方便简洁的 Medoo 操作库，它支持了很多市面上热门的数据库系统：

- MySQL, MariaDB -> php_pdo_mysql
- MSSQL (Windows) -> php_pdo_sqlsrv
- MSSQL (Liunx/UNIX) -> php_pdo_dblib / php_pdo_sqlsrv
- Oracle -> php_pdo_oci
- Oracle version 8 -> php_pdo_oci8
- SQLite -> php_pdo_sqlite
- PostgreSQL -> php_pdo_pgsql
- Sybase -> php_pdo_dblib

它们都是关系型数据库系统，但市面上还有一种非关系型的数据库类型：

- MongoDB
- Cassandra
- Memcached
- HBase
- Redis
- ...

#### 关系型 和 非关系型

写过 PHP 的应该都知道 MySQL 吧，它的数据都是存在一个个数据表中的，而数据表都是表项的，也就是说你只能在数据项的限制下才能插入数据：如果 “UserName” 的数据项类型为字符串，那你就不能直接将一个数字插入进去。—— 简单地说就是有严格限制格式。

而非关系型数据库，如 Mongodb 则不会有很严格的限制，Mongodb是文档式的储存方式，在一条数据表下，你的每条数据的格式都是可以随意变化的。比如说一个 “user_table” 两条数据有的数据项可以完全不一样，第一条有的属性，第二条可以没有。 —— 简单说就是没限制。

这两种类型的数据库不存在好与坏的区分，它们适合的是不同的项目类型。

一般正常的项目使用 MySQL 足矣，但如果你的项目数据结构是在不断变化的，就可以考虑非关系型数据库了。

## 数据库连接

LyApi 2.X 封装了特殊的数据库连接方式，你需要在 application/Config/database.php 下进行配置：

```php
return [
    "mydb" => [
        'server' => '127.0.0.1',
        'username' => 'root',
        'password' => 'root',
        'database_name' => 'mysql',
        'charset' => 'utf8',
        'database_type' => 'mysql'
    ]
];
```

你可以在这里返回所有会用到的数据库连接配置，框架会根据在使用时的选择自动连接到相应的数据库。

通过 Connector 对象取得 Medoo 连接：

```php

$ctor = new Connector(); 
$conn = $ctor->mydb;
// --------- 分割线 --------- //
$conn = Connector::connect("mydb");
```

上方有两种方法可以获取到数据库连接（前提是有相应 Key 的配置）


## 数据库操作

虽然说后端涉及到很多知识，但归根结底，我们所完成的任务基本上就是 **CURD** ！

CURD即：Create Update Read Delete (增删改查)

所以说数据库的操作在后端开发中是至关重要滴 🤠

!> 下方文档仅提供简单演示，完整文档请移步：[Medoo Database Framework](https://medoo.in/doc)

### 数据插入

数据库中的每条数据都是程序插入（创建）的，所以它至关重要！

```php
// 第一个参数为操作的表名（Medoo 所有操作第一参数都为表名）
$conn->insert("my_table",[
    
    // 第二参数则是一个数组，用于设置相关的属性
    // 如果表结构设置可空或默认值，也可以不设置
    
    'username' => 'liuzhuoer',
    'password' => '123456789'
]);
```

### 数据查询

!> 查询函数过于复杂，建议前往 [Medoo 官网](https://medoo.in/doc) 查看!

先来个基础的查询操作：

```php
// 一般会使用到的有三个参数：

// 第一个：表名
// 第二个：查询字段名（即需要的结果有哪些字段）
// 第三个：条件（相当于SQL中的 Where ）

// 查询 id 为 1 的数据，只要 username 和 email 两个值
$data = $conn->select("my_table", [
	"username",
	"email"
], [
	"id" => 1
]);
```

接下来是稍微高级的操作：


```php
// 查询字段为 * 即代表取得所有字段信息
// age[>] 相当于查询 age 大于 18 的信息

$data = $conn->select("my_table", "*", [
    "age[>]" => 18
]);
```

select 函数可能查询多条数据，如果你只要一条，请使用 **get** 函数：

```php
// 结构和上方代码一样，但本次只取得第一条符合条件的数据
$data = $conn->get("my_table", "*", [
    "age[>]" => 18
]);
```


### 更新数据

!> 更新函数过于复杂，建议前往 [Medoo 官网](https://medoo.in/doc) 查看!

```php

// 第二位参数为更新的数据
// 第三位参数同SQL Where

$conn->update("my_table",[

    // 先将 Email 更新为 mrxzx@qq.com
    // 然后 Age = Age + 1 （相当于自增）

    "email" => "mrxzx@qq.com",
    "age[+]" => 1
],[
    "username" => 'liuzhuoer'
]);
```

### 删除数据

```php

// 第二位参数相当于 SQL Where

$conn->update("my_table",[

    // 删除用户名为 liuzhuoer 的数据

    "username" => 'liuzhuoer'
]);
```