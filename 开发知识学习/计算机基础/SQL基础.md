[toc]
# SQL教程

## 关系数据库概述

### 数据模型

数据库按照数据结构来组织、存储和管理数据，实际上，数据库一共有三种模型：

* 层次模型
* 网状模型
* 关系模型

层次模型就是以“上下级”的层次关系来组织数据的一种方式，层次模型的数据结构看起来就像一颗树：

```
        ┌─────┐
        │     │
        └─────┘
           │
   ┌───────┴───────┐
   │               │
┌─────┐         ┌─────┐
│     │         │     │
└─────┘         └─────┘
   │               │
┌───┴───┐       ┌───┴───┐
│       │       │       │
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│     │ │     │ │     │ │     │
└─────┘ └─────┘ └─────┘ └─────┘
```
网状模型把每个数据节点和其他很多节点都连接起来，它的数据结构看起来就像很多城市之间的路网：

```
 ┌─────┐      ┌─────┐

┌─│     │──────│     │──┐
│ └─────┘      └─────┘  │
│    │            │     │
│    └──────┬─────┘     │
│           │           │
┌─────┐     ┌─────┐     ┌─────┐
│     │─────│     │─────│     │
└─────┘     └─────┘     └─────┘
│           │           │
│     ┌─────┴─────┐     │
│     │           │     │
│  ┌─────┐     ┌─────┐  │
└──│     │─────│     │──┘
└─────┘     └─────┘
```
关系模型把数据看作是一个二维表格，任何数据都可以通过行号+列号来唯一确定，它的数据模型看起来就是一个Excel表：
```
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
```
随着时间的推移和市场竞争，最终，基于关系模型的关系数据库获得了绝对市场份额。

为什么关系数据库获得了最广泛的应用？

因为相比层次模型和网状模型，关系模型理解和使用起来最简单。

关系数据库的关系模型是基于数学理论建立的。我们把域（Domain）定义为一组具有相同数据类型的值的集合，给定一组域D1,D2,...,Dn，它们的笛卡尔集定义为D1×D2×……×Dn={(d1,d2,...,dn)|di∈Di,i=1,2,...,n}， 而D1×D2×……×Dn的子集叫作在域D1,D2,...,Dn上的关系，表示为R(D1,D2,...,Dn)，这里的R表示...(……算了，根本讲不明白，大家也不用理解。

基于数学理论的关系模型虽然讲起来挺复杂，但是，基于日常生活的关系模型却十分容易理解。我们以学校班级为例，一个班级的学生就可以用一个表格存起来，并且定义如下：

| ID   | 姓名   | 班级ID | 性别   | 年龄   |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 小明   | 201  | M    | 9    |
| 2    | 小红   | 202  | F    | 8    |
| 3    | 小军   | 202  | M    | 8    |
| 4    | 小白   | 201  | F    | 9    |

其中，班级ID对应着另一个班级表：
| ID   | 名称    | 班主任  |
| ---- | ----- | ---- |
| 201  | 二年级一班 | 王老师  |
| 202  | 二年级二班 | 李老师  |
通过给定一个班级名称，可以查到一条班级记录，根据班级ID，又可以查到多条学生记录，这样，二维表之间就通过ID映射建立了“一对多”关系。

### 数据类型

对于一个关系表，除了定义每一列的名称外，还需要定义每一列的数据类型。关系数据库支持的标准数据类型包括数值、字符串、时间等：

| 名称           | 类型      | 说明                                       |
| ------------ | ------- | ---------------------------------------- |
| INT          | 整型      | 4字节整数类型，范围约+/-21亿                        |
| BIGINT       | 长整型     | 8字节整数类型，范围约+/-922亿亿                      |
| REAL         | 浮点型     | 4字节浮点数，范围约+/-1038                        |
| DOUBLE       | 浮点型     | 8字节浮点数，范围约+/-10308                       |
| DECIMAL(M,N) | 高精度小数   | 由用户指定精度的小数，例如，DECIMAL(20,10)表示一共20位，其中小数10位，通常用于财务计算 |
| CHAR(N)      | 定长字符串   | 存储指定长度的字符串，例如，CHAR(100)总是存储100个字符的字符串    |
| VARCHAR(N)   | 变长字符串   | 存储可变长度的字符串，例如，VARCHAR(100)可以存储0~100个字符的字符串 |
| BOOLEAN      | 布尔类型    | 存储True或者False                            |
| DATE         | 日期类型    | 存储日期，例如，2018-06-22                       |
| TIME         | 时间类型    | 存储时间，例如，12:20:59                         |
| DATETIME     | 日期和时间类型 | 存储日期+时间，例如，2018-06-22 12:20:59           |

上面的表中列举了最常用的数据类型。很多数据类型还有别名，例如，`REAL`又可以写成 `FLOAT`(24)。还有一些不常用的数据类型，例如，`TINYINT`（范围在0~255）。各数据库厂商还会支持特定的数据类型，例如 `JSON`。

选择数据类型的时候，要根据业务规则选择合适的类型。通常来说，`BIGINT`能满足整数存储的需求，`VARCHAR(N)`能满足字符串存储的需求，这两种类型是使用最广泛的。

### 主流关系数据库

目前，主流的关系数据库主要分为以下几类：

1. 商用数据库，例如：Oracle，SQL Server，DB2等；
2. 开源数据库，例如：MySQL，PostgreSQL等；
3. 桌面数据库，以微软Access为代表，适合桌面应用程序使用；
4. 嵌入式数据库，以Sqlite为代表，适合手机应用和桌面程序。

### SQL

什么是SQL？SQL是结构化查询语言的缩写，用来访问和操作数据库系统。SQL语句既可以查询数据库中的数据，也可以添加、更新和删除数据库中的数据，还可以对数据库进行管理和维护操作。不同的数据库，都支持SQL，这样，我们通过学习SQL这一种语言，就可以操作各种不同的数据库。

虽然SQL已经被ANSI组织定义为标准，不幸地是，各个不同的数据库对标准的SQL支持不太一致。并且，大部分数据库都在标准的SQL上做了扩展。也就是说，如果只使用标准SQL，理论上所有数据库都可以支持，但如果使用某个特定数据库的扩展SQL，换一个数据库就不能执行了。例如，Oracle把自己扩展的SQL称为 `PL/SQL`，Microsoft把自己扩展的SQL称为 `T-SQL`。

现实情况是，如果我们只使用标准SQL的核心功能，那么所有数据库通常都可以执行。不常用的SQL功能，不同的数据库支持的程度都不一样。而各个数据库支持的各自扩展的功能，通常我们把它们称之为“方言”。

总的来说，SQL语言定义了这么几种操作数据库的能力：

**DDL：Data Definition Language**
DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。

**DML：Data Manipulation Language**
DML为用户提供添加、删除、更新数据的能力，这些是应用程序对数据库的日常操作。

**DQL：Data Query Language**
DQL允许用户查询数据，这也是通常最频繁的数据库日常操作。

### 语法特点

SQL语言关键字不区分大小写！！！但是，针对不同的数据库，对于表名和列名，有的数据库区分大小写，有的数据库不区分大小写。同一个数据库，有的在Linux上区分大小写，有的在Windows上不区分大小写。

所以，本教程约定：SQL关键字总是大写，以示突出，表名和列名均使用小写。

## MySQL概述

MySQL是目前应用最广泛的开源关系数据库。MySQL最早是由瑞典的MySQL AB公司开发，该公司在2008年被SUN公司收购，紧接着，SUN公司在2009年被Oracle公司收购，所以MySQL最终就变成了Oracle旗下的产品。

和其他关系数据库有所不同的是，MySQL本身实际上只是一个SQL接口，它的内部还包含了多种数据引擎，常用的包括：

* InnoDB：由Innobase Oy公司开发的一款支持事务的数据库引擎，2006年被Oracle收购；
* MyISAM：MySQL早期集成的默认数据库引擎，不支持事务。

MySQL接口和数据库引擎的关系就好比某某浏览器和浏览器引擎（IE引擎或Webkit引擎）的关系。对用户而言，切换浏览器引擎不影响浏览器界面，切换MySQL引擎不影响自己写的应用程序使用MySQL的接口。

使用MySQL时，不同的表还可以使用不同的数据库引擎。如果你不知道应该采用哪种引擎，记住总是选择InnoDB就好了。

因为MySQL一开始就是开源的，所以基于MySQL的开源版本，又衍生出了各种版本：

**MariaDB**
由MySQL的创始人创建的一个开源分支版本，使用XtraDB引擎。

**Aurora**
由Amazon改进的一个MySQL版本，专门提供给在AWS托管MySQL用户，号称5倍的性能提升。

**PolarDB**
由Alibaba改进的一个MySQL版本，专门提供给在阿里云托管的MySQL用户，号称6倍的性能提升。

而MySQL官方版本又分了好几个版本：

* Community Edition：社区开源版本，免费；
* Standard Edition：标准版；
* Enterprise Edition：企业版；
* Cluster Carrier Grade Edition：集群版。

以上版本的功能依次递增，价格也依次递增。不过，功能增加的主要是监控、集群等管理功能，对于基本的SQL功能是完全一样的。

所以使用MySQL就带来了一个巨大的好处：可以在自己的电脑上安装免费的Community Edition版本，进行学习、开发、测试，部署的时候，可以选择付费的高级版本，或者云服务商提供的兼容版本，而不需要对应用程序本身做改动。

### 运行MySQL

MySQL安装后会自动在后台运行。为了验证MySQL安装是否正确，我们需要通过 `mysql`这个命令行程序来连接MySQL服务器。

在命令提示符下输入 `mysql -u root -p`，然后输入口令，如果一切正确，就会连接到MySQL服务器，同时提示符变为 `mysql>`。

输入 `exit`退出MySQL命令行。注意，MySQL服务器仍在后台运行。

## 关系模型

我们已经知道，关系数据库是建立在关系模型上的。而关系模型本质上就是若干个存储数据的二维表，可以把它们看作很多Excel表。

表的每一行称为 `记录（Record）`，记录是一个逻辑意义上的数据。

表的每一列称为 `字段（Column）`，同一个表的每一行记录都拥有相同的若干字段。

字段定义了数据类型（整型、浮点型、字符串、日期等），以及是否允许为 `NULL`。注意 `NULL`表示字段数据不存在。一个整型字段如果为 `NULL`不表示它的值为0，同样的，一个字符串型字段为 `NULL`也不表示它的值为空串 `''`。

{% note info simple %}
通常情况下，字段应该避免允许为NULL。不允许为NULL可以简化查询条件，加快查询速度，也利于应用程序读取数据后无需判断是否为NULL。
{% endnote %}

和Excel表有所不同的是，关系数据库的表和表之间需要建立“一对多”，“多对一”和“一对一”的关系，这样才能够按照应用程序的逻辑来组织和存储数据。

例如，一个班级表：

| ID   | 名称    | 班主任  |
| ---- | ----- | ---- |
| 201  | 二年级一班 | 王老师  |
| 202  | 二年级二班 | 李老师  |

每一行对应着一个班级，而一个班级对应着多个学生，所以班级表和学生表的关系就是“一对多”：

| ID   | 姓名   | 班级ID | 性别   | 年龄   |
| ---- | ---- | ---- | ---- | ---- |
| 1    | 小明   | 201  | M    | 9    |
| 2    | 小红   | 202  | F    | 8    |
| 3    | 小军   | 202  | M    | 8    |
| 4    | 小白   | 201  | F    | 9    |

反过来，如果我们先在学生表中定位了一行记录，例如 `ID=1`的小明，要确定他的班级，只需要根据他的“班级ID”对应的值201找到班级表中 `ID=201`的记录，即二年级一班。所以，学生表和班级表是“多对一”的关系。

如果我们把班级表分拆得细一点，例如，单独创建一个教师表：

| ID   | 名称   | 年龄   |
| ---- | ---- | ---- |
| A1   | 王老师  | 26   |
| A2   | 张老师  | 39   |
| A3   | 李老师  | 32   |
| A4   | 赵老师  | 27   |

班级表只存储教师ID：

| ID   | 名称    | 班主任ID |
| ---- | ----- | ----- |
| 201  | 二年级一班 | A1    |
| 202  | 二年级二班 | A3    |

这样，一个班级总是对应一个教师，班级表和教师表就是“一对一”关系。

在关系数据库中，关系是通过`主键`和`外键`来维护的。我们在后面会分别深入讲解。

### 主键

在关系数据库中，一张表中的每一行数据被称为一条记录。一条记录就是由多个字段组成的。例如，`students`表的两行记录：

| id   | class_id | name | gender | score |
| ---- | -------- | ---- | ------ | ----- |
| 1    | 1        | 小明   | M      | 90    |
| 2    | 1        | 小红   | F      | 95    |

每一条记录都包含若干定义好的字段。同一个表的所有记录都有相同的字段定义。

对于关系表，有个很重要的约束，就是任意两条记录不能重复。不能重复不是指两条记录不完全相同，而是指能够通过某个字段唯一区分出不同的记录，这个字段被称为`主键`。

例如，假设我们把 `name`字段作为主键，那么通过名字小明或小红就能唯一确定一条记录。但是，这么设定，就没法存储同名的同学了，因为插入相同主键的两条记录是不被允许的。

对主键的要求，最关键的一点是：记录一旦插入到表中，主键最好不要再修改，因为主键是用来唯一定位记录的，修改了主键，会造成一系列的影响。

由于主键的作用十分重要，如何选取主键会对业务开发产生重要影响。如果我们以学生的身份证号作为主键，似乎能唯一定位记录。然而，身份证号也是一种业务场景，如果身份证号升位了，或者需要变更，作为主键，不得不修改的时候，就会对业务产生严重影响。

所以，选取主键的一个基本原则是：不使用任何业务相关的字段作为主键。

因此，身份证号、手机号、邮箱地址这些看上去可以唯一的字段，均不可用作主键。

作为主键最好是完全业务无关的字段，我们一般把这个字段命名为 `id`。常见的可作为id字段的类型有：

* 自增整数类型：数据库会在插入数据时自动为每一条记录分配一个自增整数，这样我们就完全不用担心主键重复，也不用自己预先生成主键；
* 全局唯一GUID类型：使用一种全局唯一的字符串作为主键，类似8f55d96b-8acc-4636-8cb8-76bf8abc2f57。GUID算法通过网卡MAC地址、时间戳和随机数保证任意计算机在任意时间生成的字符串都是不同的，大部分编程语言都内置了GUID算法，可以自己预算出主键。

对于大部分应用来说，通常自增类型的主键就能满足需求。我们在 `students`表中定义的主键也是 `BIGINT NOT NULL AUTO_INCREMENT`类型。

>如果使用INT自增类型，那么当一张表的记录数超过2147483647（约21亿）时，会达到上限而出错。使用BIGINT自增类型则可以最多约922亿亿条记录。

#### 联合主键

关系数据库实际上还允许通过多个字段唯一标识记录，即两个或更多的字段都设置为主键，这种主键被称为联合主键。

对于联合主键，允许一列有重复，只要不是所有主键列都重复即可：

| id_num | id_type | other columns... |
| ------ | ------- | ---------------- |
| 1      | A       | ...              |
| 2      | A       | ...              |
| 2      | B       | ...              |

如果我们把上述表的 `id_num`和 `id_type`这两列作为联合主键，那么上面的3条记录都是允许的，因为没有两列主键组合起来是相同的。

没有必要的情况下，我们尽量不使用联合主键，因为它给关系表带来了复杂度的上升。

### 外键

当我们用主键唯一标识记录时，我们就可以在 `students`表中确定任意一个学生的记录：

| id   | name | other columns... |
| ---- | ---- | ---------------- |
| 1    | 小明   | ...              |
| 2    | 小红   | ...              |

我们还可以在 `classes`表中确定任意一个班级记录：

| id   | name | other columns... |
| ---- | ---- | ---------------- |
| 1    | 一班   | ...              |
| 2    | 二班   | ...              |

但是我们如何确定 `students`表的一条记录，例如，`id=1`的小明，属于哪个班级呢？

由于一个班级可以有多个学生，在关系模型中，这两个表的关系可以称为“一对多”，即一个 `classes`的记录可以对应多个 `students`表的记录。

为了表达这种一对多的关系，我们需要在students表中加入一列 `class_id`，让它的值与 `classes`表的某条记录相对应：

| id   | class_id | name | other columns... |
| ---- | -------- | ---- | ---------------- |
| 1    | 1        | 小明   | ...              |
| 2    | 1        | 小红   | ...              |
| 5    | 2        | 小白   | ...              |

这样，我们就可以根据 `class_id`这个列直接定位出一个 `students`表的记录应该对应到classes的哪条记录。

例如：

* 小明的`class_id`是`1`，因此，对应的`classes`表的记录是`id=1`的一班；
* 小红的`class_id`是`1`，因此，对应的`classes`表的记录是`id=1`的一班；
* 小白的`class_id`是`2`，因此，对应的`classes`表的记录是`id=2`的二班。

在 `students`表中，通过 `class_id`的字段，可以把数据与另一张表关联起来，这种列称为`外键`。

外键并不是通过列名实现的，而是通过定义外键约束实现的：

```sql
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
```

其中，外键约束的名称 `fk_class_id`可以任意，`FOREIGN KEY (class_id)`指定了 `class_id`作为外键，`REFERENCES classes (id)`指定了这个外键将关联到 `classes`表的 `id`列（即 `classes`表的主键）。

通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果 `classes`表不存在 `id=99`的记录，`students`表就无法插入 `class_id=99`的记录。

由于外键约束会降低数据库的性能，大部分互联网应用程序为了追求速度，并不设置外键约束，而是仅靠应用程序自身来保证逻辑的正确性。这种情况下，`class_id`仅仅是一个普通的列，只是它起到了外键的作用而已。

要删除一个外键约束，也是通过 `ALTER TABLE`实现的：

```sql
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
```

注意：删除外键约束并没有删除外键这一列。删除列是通过 `DROP COLUMN ...`实现的。

#### 多对多

通过一个表的外键关联到另一个表，我们可以定义出一对多关系。有些时候，还需要定义“多对多”关系。例如，一个老师可以对应多个班级，一个班级也可以对应多个老师，因此，班级表和老师表存在多对多关系。

多对多关系实际上是通过两个一对多关系实现的，即通过一个中间表，关联两个一对多关系，就形成了多对多关系：

`teachers`表：

| id   | name |
| ---- | ---- |
| 1    | 张老师  |
| 2    | 王老师  |
| 3    | 李老师  |
| 4    | 赵老师  |

`classes`表：

| id   | name |
| ---- | ---- |
| 1    | 一班   |
| 2    | 二班   |

中间表 `teacher_class`关联两个一对多关系：

| id   | teacher_id | class_id |
| ---- | ---------- | -------- |
| 1    | 1          | 1        |
| 2    | 1          | 2        |
| 3    | 2          | 1        |
| 4    | 2          | 2        |
| 5    | 3          | 1        |
| 6    | 4          | 2        |

通过中间表 `teacher_class`可知 `teachers`到 `classes`的关系：

* `id=1`的张老师对应`id=1,2`的一班和二班；
* `id=2`的王老师对应`id=1,2`的一班和二班；
* `id=3`的李老师对应`id=1`的一班；
* `id=4`的赵老师对应`id=2`的二班。

同理可知 `classes`到 `teachers`的关系：

* `id=1`的一班对应`id=1,2,3`的张老师、王老师和李老师；
* `id=2`的二班对应`id=1,2,4`的张老师、王老师和赵老师；

因此，通过中间表，我们就定义了一个“多对多”关系。

#### 一对一

一对一关系是指，一个表的记录对应到另一个表的唯一一个记录。

例如，`students`表的每个学生可以有自己的联系方式，如果把联系方式存入另一个表 `contacts`，我们就可以得到一个“一对一”关系：

| id   | student_id | mobile      |
| ---- | ---------- | ----------- |
| 1    | 1          | 135xxxx6300 |
| 2    | 2          | 138xxxx2209 |
| 3    | 5          | 139xxxx8086 |

有细心的童鞋会问，既然是一对一关系，那为啥不给 `students`表增加一个 `mobile`列，这样就能合二为一了？

如果业务允许，完全可以把两个表合为一个表。但是，有些时候，如果某个学生没有手机号，那么，`contacts`表就不存在对应的记录。实际上，一对一关系准确地说，是 `contacts`表一对一对应 `students`表。

还有一些应用会把一个大表拆成两个一对一的表，目的是把经常读取和不经常读取的字段分开，以获得更高的性能。例如，把一个大的用户表分拆为用户基本信息表 `user_info`和用户详细信息表 `user_profiles`，大部分时候，只需要查询 `user_info`表，并不需要查询 `user_profiles`表，这样就提高了查询速度。

### 索引

在关系数据库中，如果有上万甚至上亿条记录，在查找记录的时候，想要获得非常快的速度，就需要使用索引。

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。

例如，对于 `students`表：

| id                                      | class_id | name | gender | score |
| --------------------------------------- | -------- | ---- | ------ | ----- |
| 1                                       | 1        | 小明   | M      | 90    |
| 2                                       | 1        | 小红   | F      | 95    |
| 3                                       | 1        | 小军   | M      | 88    |
| 如果要经常根据 `score`列进行查询，就可以对 `score`列创建索引： |          |      |        |       |

```sql
ALTER TABLE students
ADD INDEX idx_score (score);
```

使用 `ADD INDEX idx_score (score)`就创建了一个名称为 `idx_score`，使用列 `score`的索引。索引名称是任意的，索引如果有多列，可以在括号里依次写上，例如：

```sql
ALTER TABLE students
ADD INDEX idx_name_score (name, score);
```

索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。反过来，如果记录的列存在大量相同的值，例如 `gender`列，大约一半的记录值是 `M`，另一半是 `F`，因此，对该列创建索引就没有意义。

可以对一张表创建多个索引。索引的优点是提高了查询效率，缺点是在插入、更新和删除记录时，需要同时修改索引，因此，索引越多，插入、更新和删除记录的速度就越慢。

对于主键，关系数据库会自动对其创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

#### 唯一索引

在设计关系数据表的时候，看上去唯一的列，例如身份证号、邮箱地址等，因为他们具有业务含义，因此不宜作为主键。

但是，这些列根据业务要求，又具有唯一性约束：即不能出现两条记录存储了同一个身份证号。这个时候，就可以给该列添加一个唯一索引。例如，我们假设 `students`表的 `name`不能重复：

```sql
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);
```

通过 `UNIQUE`关键字我们就添加了一个唯一索引。

也可以只对某一列添加一个唯一约束而不创建唯一索引：

```sql
ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name);
```

这种情况下，`name`列没有索引，但仍然具有唯一性保证。

无论是否创建索引，对于用户和应用程序来说，使用关系数据库不会有任何区别。这里的意思是说，当我们在数据库中查询时，如果有相应的索引可用，数据库系统就会自动使用索引来提高查询效率，如果没有索引，查询也能正常执行，只是速度会变慢。因此，索引可以在使用数据库的过程中逐步优化。

## 基础语法

模式定义了数据如何存储、存储什么样的数据以及数据如何分解等信息，数据库和表都有模式。

主键的值不允许修改，也不允许复用（不能将已经删除的主键值赋给新数据行的主键）。

SQL（Structured Query Language)，标准 SQL 由 ANSI 标准委员会管理，从而称为 ANSI SQL。各个 DBMS 都有自己的实现，如 PL/SQL、Transact-SQL 等。

SQL 语句不区分大小写，但是数据库表名、列名和值是否区分依赖于具体的 DBMS 以及配置。

SQL 支持以下三种注释：

```sql
## 注释
SELECT *
FROM mytable; -- 注释
/* 注释1
   注释2 */
```

数据库创建与使用：

```sql
CREATE DATABASE test;
USE test;
```

### 创建表

```sql
CREATE TABLE mytable (
  # int 类型，不为空，自增
  id INT NOT NULL AUTO_INCREMENT,
  # int 类型，不可为空，默认值为 1，不为空
  col1 INT NOT NULL DEFAULT 1,
  # 变长字符串类型，最长为 45 个字符，可以为空
  col2 VARCHAR(45) NULL,
  # 日期类型，可为空
  col3 DATE NULL,
  # 设置主键为 id
  PRIMARY KEY (`id`));
```

### 修改表

添加列

```sql
ALTER TABLE mytable
ADD col CHAR(20);
```

删除列

```sql
ALTER TABLE mytable
DROP COLUMN col;
```

删除表

```sql
DROP TABLE mytable;
```

## 查询数据

**DISTINCT**
相同值只会出现一次。它作用于所有列，也就是说所有列的值都相同才算相同。

```sql
SELECT DISTINCT col1, col2
FROM mytable;
```

**LIMIT**
限制返回的行数。可以有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。

返回前 5 行：

```sql
SELECT *
FROM mytable
LIMIT 5;
```

```sql
SELECT *
FROM mytable
LIMIT 0, 5;
```

返回第 3 ~ 5 行：

```sql
SELECT *
FROM mytable
LIMIT 2, 3;
```

### 基本查询

要查询数据库表的数据，我们使用如下的SQL语句：

> SELECT * FROM <表名>

假设表名是 `students`，要查询 `students`表的所有行，我们用如下SQL语句：

```sql
SELECT * FROM students;
```

使用 `SELECT * FROM students`时，`SELECT`是关键字，表示将要执行一个查询，`*`表示“所有列”，`FROM`表示将要从哪个表查询，本例中是 `students`表。

该SQL将查询出 `students`表的所有数据。注意：查询结果也是一个二维表，它包含列名和每一行的数据。

`SELECT`语句其实并不要求一定要有 `FROM`子句。我们来试试下面的 `SELECT`语句：

```sql
SELECT 100+200;
```

上述查询会直接计算出表达式的结果。虽然 `SELECT`可以用作计算，但它并不是SQL的强项。但是，不带 `FROM`子句的 `SELECT`语句有一个有用的用途，就是用来判断当前到数据库的连接是否有效。许多检测工具会执行一条 `SELECT 1;`来测试数据库连接。

### 条件查询

使用 `SELECT * FROM <表名>`可以查询到一张表的所有记录。但是，很多时候，我们并不希望获得所有记录，而是根据条件选择性地获取指定条件的记录，例如，查询分数在80分以上的学生记录。在一张表有数百万记录的情况下，获取所有记录不仅费时，还费内存和网络带宽。

SELECT语句可以通过 `WHERE`条件来设定查询条件，查询结果是满足查询条件的记录。例如，要指定条件“分数在80分或以上的学生”，写成 `WHERE`条件就是 `SELECT * FROM students WHERE score >= 80。`

其中，`WHERE`关键字后面的 `score >= 80`就是条件。`score`是列名，该列存储了学生的成绩，因此，`score >= 80`就筛选出了指定条件的记录

因此，条件查询的语法就是：

> SELECT * FROM <表名> WHERE <条件表达式>

条件表达式可以用 `<条件1> AND <条件2>`表达满足条件1并且满足条件2。例如，符合条件“分数在80分或以上”，并且还符合条件“男生”，把这两个条件写出来：

* 条件1：根据`score`列的数据判断：`score >= 80`；
* 条件2：根据`gender`列的数据判断：`gender = 'M'`，注意`gender`列存储的是字符串，需要用单引号括起来。

就可以写出 `WHERE`条件 `：score >= 80 AND gender = 'M'`：

```sql
SELECT * FROM students WHERE score >= 80 AND gender = 'M';
```

第二种条件是 `<条件1> OR <条件2>`，表示满足条件1或者满足条件2。例如，把上述 `AND`查询的两个条件改为 `OR`，查询结果就是“分数在80分或以上”或者“男生”，满足任意之一的条件即选出该记录：

```sql
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
```

很显然 `OR`条件要比 `AND`条件宽松，返回的符合条件的记录也更多。

第三种条件是 `NOT <条件>`，表示“不符合该条件”的记录。例如，写一个“不是2班的学生”这个条件，可以先写出“是2班的学生”：`class_id = 2`，再加上 `NOT`：`NOT class_id = 2`：

```sql
SELECT * FROM students WHERE NOT class_id = 2;
```

上述 `NOT`条件 `NOT class_id = 2`其实等价于 `class_id <> 2`，因此，`NOT`查询不是很常用。

要组合三个或者更多的条件，就需要用小括号 `()`表示如何进行条件运算。例如，编写一个复杂的条件：分数在80以下或者90以上，并且是男生：

```sql
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
```

如果不加括号，条件运算按照 `NOT`、`AND`、OR的优先级进行，即NOT优先级最高，其次是AND，最后是OR。加上括号可以改变优先级。

### 通配符

| 条件         | 表达式举例1          | 表达式举例2           | 说明                                  |
| ---------- | --------------- | ---------------- | ----------------------------------- |
| 使用LIKE判断相似 | name LIKE 'ab%' | name LIKE '%bc%' | %表示任意字符，例如'ab%'将匹配'ab'，'abc'，'abcd' |

通配符也是用在过滤语句中，但它只能用于文本字段。

* % 匹配 >=0 个任意字符；
* _ 匹配 ==1 个任意字符；
* `[ ]`可以匹配集合内的字符，例如 `[ab]` 将匹配字符 a 或者 b。用脱字符 `^` 可以对其进行否定，也就是不匹配集合内的字符。

使用 `Like` 来进行通配符匹配。

```sql
SELECT *
FROM mytable
WHERE col LIKE '[^AB]%'; -- 不以 A 和 B 开头的任意文本
```

不要滥用通配符，通配符位于开头处匹配会非常慢。

### 计算字段

在数据库服务器上完成数据的转换和格式化的工作往往比客户端上快得多，并且转换和格式化后的数据量更少的话可以减少网络通信量。

计算字段通常需要使用 `AS` 来取别名，否则输出的时候字段名为计算表达式。

```sql
SELECT col1 * col2 AS alias
FROM mytable;
```

`CONCAT()` 用于连接两个字段。许多数据库会使用空格把一个值填充为列宽，因此连接的结果会出现一些不必要的空格，使用 `TRIM()` 可以去除首尾空格。

```sql
SELECT CONCAT(TRIM(col1), '(', TRIM(col2), ')') AS concat_col
FROM mytable;
```

### 投影查询

使用 `SELECT * FROM <表名> WHERE <条件>`可以选出表中的若干条记录。我们注意到返回的二维表结构和原表是相同的，即结果集的所有列与原表的所有列都一一对应。

如果我们只希望返回某些列的数据，而不是所有列的数据，我们可以用 `SELECT 列1, 列2, 列3 FROM ...，`让结果集仅包含指定列。这种操作称为投影查询。

例如，从 `students`表中返回 `id`、`score`和 `name`这三列：

```sql
SELECT id, score, name FROM students;
```

这样返回的结果集就只包含了我们指定的列，并且，结果集的列的顺序和原表可以不一样。

使用 `SELECT 列1, 列2, 列3 FROM ...`时，还可以给每一列起个别名，这样，结果集的列名就可以与原表的列名不同。它的语法是 `SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...`。

例如，以下 `SELECT`语句将列名 `score`重命名为 `points`，而 `id`和 `name`列名保持不变：

```sql
SELECT id, score points, name FROM students;
```

投影查询同样可以接 `WHERE`条件，实现复杂的查询：

```sql
-- 使用投影查询+WHERE条件：
SELECT id, score points, name FROM students WHERE gender = 'M';
```

### 排序

我们使用 `SELECT`查询时，细心的读者可能注意到，查询结果集通常是按照 `id`排序的，也就是根据主键排序。这也是大部分数据库的做法。如果我们要根据其他条件排序怎么办？可以加上 `ORDER BY`子句。例如按照成绩 `从低到高`进行排序：

```sql
SELECT id, name, gender, score FROM students ORDER BY score;
```

如果要反过来，按照成绩 `从高到底`排序，我们可以加上 `DESC`表示“倒序”：

```sql
-- 按score从高到低
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```

如果 `score`列有相同的数据，要进一步排序，可以继续添加列名。例如，使用 `ORDER BY score DESC, gender`表示先按 `score`列倒序，如果有相同分数的，再按 `gender`列排序：

```sql
-- 按score, gender排序:
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```

默认的排序规则是 `ASC`：“升序”，即从小到大。`ASC`可以省略，即 `ORDER BY score ASC`和 `ORDER BY score`效果一样。

如果有 `WHERE`子句，那么 `ORDER BY`子句要放到 `WHERE`子句后面。例如，查询一班的学生成绩，并按照倒序排序：

```sql
-- 带WHERE条件的ORDER BY:
SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;
```

这样，结果集仅包含符合 `WHERE`条件的记录，并按照 `ORDER BY`的设定排序。

### 分页查询

使用SELECT查询时，如果结果集数据量很大，比如几万行数据，放在一个页面显示的话数据量太大，不如分页显示，每次显示100条。

要实现分页功能，实际上就是从结果集中显示第 `1~100`条记录作为第1页，显示第 `101~200`条记录作为第2页，以此类推。

因此，分页实际上就是从结果集中“截取”出第M~N条记录。这个查询可以通过 `LIMIT <N-M> OFFSET <M>`子句实现。我们先把所有学生按照成绩从高到低进行排序：

```sql
-- 按score从高到低
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```

现在，我们把结果集分页，每页3条记录。要获取第1页的记录，可以使用 `LIMIT 3 OFFSET 0`：

```sql
-- 查询第1页
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
```

上述查询 `LIMIT 3 OFFSET 0`表示，对结果集从0号记录开始，最多取3条。注意SQL记录集的索引从0开始。

如果要查询第2页，那么我们只需要“跳过”头3条记录，也就是对结果集从3号记录开始查询，把 `OFFSET`设定为 `3`：

```sql
-- 查询第2页
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 3;
```

类似的，查询第3页的时候，`OFFSET`应该设定为 `6`:

```sql
-- 查询第3页
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 6;
```

查询第4页的时候，`OFFSET`应该设定为 `9`:

```sql
-- 查询第4页
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 9;
```

由于第4页只有1条记录，因此最终结果集按实际数量1显示。`LIMIT 3`表示的意思是“最多3条记录”。

可见，分页查询的关键在于，首先要确定每页需要显示的结果数量 `pageSize`（这里是3），然后根据当前页的索引 `pageIndex`（从1开始），确定 `LIMIT`和 `OFFSET`应该设定的值：

* `LIMIT`总是设定为`pageSize`；
* `OFFSET`计算公式为`pageSize * (pageIndex - 1)`。

这样就能正确查询出第N页的记录集。

如果原本记录集一共就10条记录，但我们把 `OFFSET`设置为20，会得到什么结果呢？

```sql
-- OFFSET设定为20
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 20;
```

`OFFSET`超过了查询的最大数量并不会报错，而是得到一个空的结果集。

#### 注意

`OFFSET`是可选的，如果只写 `LIMIT 15`，那么相当于 `LIMIT 15 OFFSET 0`。

在MySQL中，`LIMIT 15 OFFSET 30`还可以简写成 `LIMIT 30, 15`。

使用 `LIMIT <M> OFFSET <N>`分页时，随着 `N`越来越大，查询效率也会越来越低。

### 聚合查询

如果我们要统计一张表的数据量，例如，想查询 `students`表一共有多少条记录，难道必须用 `SELECT * FROM students`查出来然后再数一数有多少行吗？

这个方法当然可以，但是比较弱智。对于统计总数、平均数这类计算，SQL提供了专门的聚合函数，使用聚合函数进行查询，就是聚合查询，它可以快速获得结果。

仍然以查询 `students`表一共有多少条记录为例，我们可以使用SQL内置的 `COUNT()`函数查询：

```sql
-- 使用聚合查询:
SELECT COUNT(*) FROM students;
```

`COUNT(*)`表示查询所有列的行数，要注意聚合的计算结果虽然是一个数字，但查询的结果仍然是一个二维表，只是这个二维表只有一行一列，并且列名是 `COUNT(*)`。

通常，使用聚合查询时，我们应该给列名设置一个别名，便于处理结果：

```sql
-- 使用聚合查询并设置结果集的列名为num:
SELECT COUNT(*) num FROM students;
```

`COUNT(*)`和 `COUNT(id)`实际上是一样的效果。另外注意，聚合查询同样可以使用 `WHERE`条件，因此我们可以方便地统计出有多少男生、多少女生、多少80分以上的学生等：

```sql
-- 使用聚合查询并设置WHERE条件:
SELECT COUNT(*) boys FROM students WHERE gender = 'M';
```

除了 `COUNT()`函数外，SQL还提供了如下聚合函数：

| 函数   | 说明                  |
| ---- | ------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值           |
| MIN  | 计算某一列的最小值           |

注意，`MAX()`和 `MIN()`函数并不限于数值类型。如果是字符类型，`MAX()`和 `MIN()`会返回 `排序最后`和 `排序最前`的字符。

要统计男生的平均成绩，我们用下面的聚合查询：

```sql
-- 使用聚合查询计算男生平均成绩:
SELECT AVG(score) average FROM students WHERE gender = 'M';
```

要特别注意：如果聚合查询的 `WHERE`条件没有匹配到任何行，`COUNT()`会返回 `0`，而 `SUM()`、`AVG()`、`MAX()`和 `MIN()`会返回 `NULL`：

```sql
-- WHERE条件gender = 'X'匹配不到任何行:
SELECT AVG(score) average FROM students WHERE gender = 'X';
```

每页3条记录，如何通过聚合查询获得总页数？

```sql
SELECT CEILING(COUNT(*) / 3) FROM students;
```

#### 分组

如果我们要统计一班的学生数量，我们知道，可以用 `SELECT COUNT(*) num FROM students WHERE class_id = 1`;。如果要继续统计二班、三班的学生数量，难道必须不断修改 `WHERE`条件来执行 `SELECT`语句吗？

对于聚合查询，SQL还提供了“分组聚合”的功能。我们观察下面的聚合查询：

```sql
SELECT COUNT(*) num FROM students GROUP BY class_id;
```

执行这个查询，`COUNT()`的结果不再是一个，而是3个，这是因为，`GROUP BY`子句指定了按 `class_id`分组，因此，执行该 `SELECT`语句时，会把 `class_id`相同的列先分组，再分别计算，因此，得到了3行结果。

但是这3行结果分别是哪三个班级的，不好看出来，所以我们可以把 `class_id`列也放入结果集中：

```sql
-- 按class_id分组:
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
```

这下结果集就可以一目了然地看出各个班级的学生人数。我们再试试把 `name`放入结果集：

```sql
-- 按class_id分组:
SELECT name, class_id, COUNT(*) num FROM students GROUP BY class_id;
```

不出意外，执行这条查询我们会得到一个语法错误，因为在任意一个分组中，只有 `class_id`都相同，`name`是不同的，SQL引擎不能把多个 `name`的值放入一行记录中。因此，聚合查询的列中，只能放入分组的列。

{% note warning sample %}
注意：AlaSQL并没有严格执行SQL标准，上述SQL在浏览器可以正常执行，但是在MySQL、Oracle等环境下将报错，请自行在MySQL中测试。
{% endnote %}

也可以使用多个列进行分组。例如，我们想统计各班的男生和女生人数：

```sql
-- 按class_id, gender分组:
SELECT class_id, gender, COUNT(*) num FROM students GROUP BY class_id, gender;
```

上述查询结果集一共有6条记录，分别对应各班级的男生和女生人数。

WHERE 过滤行，HAVING 过滤分组，行过滤应当先于分组过滤。

```sql
SELECT col, COUNT(*) AS num
FROM mytable
WHERE col > 2
GROUP BY col
HAVING num >= 2;
```

分组规定：

* GROUP BY 子句出现在 WHERE 子句之后，ORDER BY 子句之前；
* 除了汇总字段外，SELECT 语句中的每一字段都必须在 GROUP BY 子句中给出；
* NULL 的行会单独分为一组；
* 大多数 SQL 实现不支持 GROUP BY 列具有可变长度的数据类型。

### 嵌套查询

子查询中只能返回一个字段的数据。

可以将子查询的结果作为 WHRER 语句的过滤条件：

```sql
SELECT *
FROM mytable1
WHERE col1 IN (SELECT col2
               FROM mytable2);
```

#### in操作符

IN 操作符允许我们在 WHERE 子句中规定多个值。

SQL IN 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...)
```

原始的表 (在实例中使用：)
Persons 表:

| Id   | LastName | FirstName | Address        | City     |
| ---- | -------- | --------- | -------------- | -------- |
| 1    | Adams    | John      | Oxford Street  | London   |
| 2    | Bush     | George    | Fifth Avenue   | New York |
| 3    | Carter   | Thomas    | Changan Street | Beijing  |

IN 操作符实例

现在，我们希望从上表中选取姓氏为 Adams 和 Carter 的人：

我们可以使用下面的 `SELECT` 语句：

```sql
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```

结果集：
| Id   | LastName | FirstName | Address        | City    |
| ---- | -------- | --------- | -------------- | ------- |
| 1    | Adams    | John      | Oxford Street  | London  |
| 3    | Carter   | Thomas    | Changan Street | Beijing |

#### 带有比较运算符的子查询

带有比较运算符的子查询是指父查询与子查询之间用比较运算符进行连接。当用户能确切知道内层查询返回的是单个值时，可以用 >、<、=、>=、<=、!=、或<>等比较运算符。

找出每个学生超过他自己选修课程平均成绩的课程号

```sql
SELECT Sno,Cno
FROM SC X
WHERE Grade >=(SELECT AVG(Grade)
               FROM SC y
               WHERE y.Sno=x.Sno);
```

#### 带有ANY（SOME）或ALL谓词的子查询

子查询返回单值时可以用比较运算符，但返回多值时要用ANY（有的系统用SOME）或ALL谓词修饰符。而使用ANY或ALL谓词时则必须同时使用比较运算符。其语义如下：

|`>ANY`|大于子查询结果中的某个值|
|`>ALL`|大于子查询结果中的所有值|
|`<ANY`|小于子查询结果中的某个值|
|`<ALL`|小于子查询结果中的所有值|
|`>=ANY`|大于等于子查询结果中的某个值|
|`>=ALL`|大于等于子查询结果中的所有值|
|`<=ALL`|小于等于子查询结果中的所有值|
|`<=ANY`|大于等于子查询结果中的某个值|
|`=ANY`|等于子查询结果中的某个值|
|`=ALL`|等于子查询结果中的所有值（通常没有实际意义）|
|`!=（或<>）ANY`|不等于子查询结果中的某个值|
|`!=（或<>）ALL`|不等于子查询结果中的任何一个值|

查询非计算机科学系中比计算机科学系任意一个学生年龄小的学生姓名和年龄

```sql
SELECT Sname,Sage
FROM Student
WHERE Sage<ANY (SELECT Sage
                FROM Student
                WHERE Sdept='CS')
AND Sdept <> 'CS';
```

查询非计算机科学系中比计算机科学系所有学生年龄都小的学生姓名和年龄

```sql
SELECT Sname,Sage
FROM Student
WHERE Sage<ALL
          (SELECT Sage
           FROM Student
           WHERE Sdept='CS')
AND Sdept <> 'CS';
```

提示：本查询同样可以用聚集函数实现

```sql
SELECT Sname,Sage
FROM Student
WHERE Sage <
    (SELECT MIN(Sage)
     FROM Student
     WHERE Sdept='CS')
AND Sdept <>'CS';
```

在此把ANY、ALL与聚集函数的对应关系表示如下

![](https://images2017.cnblogs.com/blog/922928/201801/922928-20180106224147987-155828251.png)

#### 带有 EXISTS 谓词的子查询

带有EXISTS 谓词的子查询不返回任何数据，只产生逻辑真值“true”或逻辑假值“false”。

查询所有选修了1号课程的学生姓名

```sql
SELECT Sname
FROM Student
WHERE EXISTS
    (SELECT *
     FROM SC
     WHERE Sno=Student.Sno AND Cno='1');
```

使用存在量词EXISTS后，若内层查询结果为空，则外层的WHERE子句返回真值，否则返回假值。

查询没有选修1号课程的学生姓名

```sql
SELECT Sname
FROM Student
WHERE NOT EXISTS
    (SELECT *
     FROM SC
     WHERE Sno=Student.Sno AND Cno='1');
```

查询选修了全部课程的学生姓名

由于没有全称量词，可将题目的意思转换成等价的用存在量词的形式：查询这样的学生，没有一门课程是他不选修的。

```sql
SELECT Sname
FROM Student
WHERE NOT EXISTS
    (SELECT *
     FROM Course
     WHERE NOT EXISTS
        (SELECT *
         FROM SC
         WHERE Sno=Student.Sno
            AND Cno=Course.Cno));
```

查询至少选修了学生201215122选修的全部课程的学生号码

```sql
SELECT DISTINCT Sno
FROM SC SCX
WHERE NOT EXISTS
    (SELECT *
     FROM SC SCY
     WHERE SCY.Sno='201215122' AND
        NOT EXISTS
        (SELECT *
         FROM SC SCZ
         WHERE SCZ.Sno=SCX.Sno AND
         SCZ.Cno=SCY.Cno));
```

#### 子查询

下面的语句可以检索出客户的订单数量，子查询语句会对第一个查询检索出的每个客户执行一次：

```sql
SELECT cust_name, (SELECT COUNT(*)
                   FROM Orders
                   WHERE Orders.cust_id = Customers.cust_id)
                   AS orders_num
FROM Customers
ORDER BY cust_name;
```

### 多表查询

SELECT查询不但可以从一张表查询数据，还可以从多张表同时查询数据。查询多张表的语法是：`SELECT * FROM <表1> <表2>`。

例如，同时从 `students`表和 `classes`表的“乘积”，即查询数据，可以这么写：

```sql
-- FROM students, classes:
SELECT * FROM students, classes;
```

这种一次查询两个表的数据，查询的结果也是一个二维表，它是 `students`表和 `classes`表的“乘积”，即 `students`表的每一行与 `classes`表的每一行都两两拼在一起返回。结果集的列数是 `students`表和 `classes`表的列数之和，行数是 `students`表和 `classes`表的行数之积。

这种多表查询又称笛卡尔查询，使用笛卡尔查询时要非常小心，由于结果集是目标表的行数乘积，对两个各自有100行记录的表进行笛卡尔查询将返回1万条记录，对两个各自有1万行记录的表进行笛卡尔查询将返回1亿条记录。

你可能还注意到了，上述查询的结果集有两列 `id`和两列 `name`，两列id是因为其中一列是 `students`表的 `id`，而另一列是 `classes`表的 `id`，但是在结果集中，不好区分。两列 `name`同理

要解决这个问题，我们仍然可以利用投影查询的“设置列的别名”来给两个表各自的 `id`和 `name`列起别名：

```sql
-- set alias:
SELECT
    students.id sid,
    students.name,
    students.gender,
    students.score,
    classes.id cid,
    classes.name cname
FROM students, classes;
```

注意，多表查询时，要使用 `表名.列名`这样的方式来引用列和设置别名，这样就避免了结果集的列名重复问题。但是，用 `表名.列名`这种方式列举两个表的所有列实在是很麻烦，所以SQL还允许给表设置一个别名，让我们在投影查询中引用起来稍微简洁一点：

```sql
-- set table alias:
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;
```

注意到 `FROM`子句给表设置别名的语法是 `FROM <表名1> <别名1>`, `<表名2> <别名2>`。这样我们用别名 `s`和 `c`分别表示 `students`表和 `classes`表。

多表查询也是可以添加 `WHERE`条件的，我们来试试：

```sql
-- set where clause:
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;
```

这个查询的结果集每行记录都满足条件 `s.gender = 'M'`和 `c.id = 1`。添加WHERE条件后结果集的数量大大减少了。

### 连接查询

连接查询是另一种类型的多表查询。连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

例如，我们想要选出 `students`表的所有学生信息，可以用一条简单的SELECT语句完成：

```sql
-- 选出所有学生
SELECT s.id, s.name, s.class_id, s.gender, s.score FROM students s;
```

但是，假设我们希望结果集同时包含所在班级的名称，上面的结果集只有 `class_id`列，缺少对应班级的 `name`列。

现在问题来了，存放班级名称的 `name`列存储在 `classes`表中，只有根据 `students`表的 `class_id`，找到 `classes`表对应的行，再取出 `name`列，就可以获得班级名称。

这时，连接查询就派上了用场。我们先使用最常用的一种内连接——`INNER JOIN`来实现：

#### 内连接

可以不明确使用 INNER JOIN，而使用普通查询并在 WHERE 中将两个表中要连接的列用等值方法连接起来。

```sql
SELECT A.value, B.value
FROM tablea AS A, tableb AS B
WHERE A.key = B.key;
```

内连接又称等值连接，使用 INNER JOIN 关键字。

```sql
-- 选出所有学生，同时返回班级名称
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;
```

注意 `INNER JOIN`查询的写法是：

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上WHERE子句、ORDER BY等子句。

使用别名不是必须的，但可以更好地简化查询语句。

那什么是 `内连接（INNER JOIN）`呢？先别着急，有 `内连接（INNER JOIN）`就有 `外连接（OUTER JOIN）`。我们把内连接查询改成外连接查询，看看效果：

#### 外连接

```sql
-- 使用OUTER JOIN
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
RIGHT OUTER JOIN classes c
ON s.class_id = c.id;
```

执行上述 `RIGHT OUTER JOIN`可以看到，和 `INNER JOIN`相比，`RIGHT OUTER JOIN`多了一行，多出来的一行是“四班”，但是，学生相关的列如 `name`、`gender`、`score`都为 `NULL`。

这也容易理解，因为根据 `ON`条件 `s.class_id = c.id`，`classes`表的 `id=4`的行正是“四班”，但是，`students`表中并不存在 `class_id=4`的行。

有 `RIGHT OUTER JOIN`，就有 `LEFT OUTER JOIN`，以及 `FULL OUTER JOIN`。它们的区别是：

`INNER JOIN`只返回同时存在于两张表的行数据，由于 `students`表的 `class_id`包含 `1`，`2`，`3`，`classes`表的id包含 `1`，`2`，`3`，`4`，所以，`INNER JOIN`根据条件 `s.class_id = c.id`返回的结果集仅包含 `1，2，3`。

`RIGHT OUTER JOIN`返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以 `NULL`填充剩下的字段。

`LEFT OUTER JOIN`则返回左表都存在的行。如果我们给 `students`表增加一行，并添加 `class_id=5`，由于 `classes`表并不存在 `id=5`的行，所以，`LEFT OUTER JOIN`的结果会增加一行，对应的 `class_name`是 `NULL`：

```sql
-- 先增加一列class_id=5:
INSERT INTO students (class_id, name, gender, score) values (5, '新生', 'M', 88);
-- 使用LEFT OUTER JOIN
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
LEFT OUTER JOIN classes c
ON s.class_id = c.id;
```

最后，我们使用 `FULL OUTER JOIN`，它会把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为 `NULL`：

```sql
-- 使用FULL OUTER JOIN
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
FULL OUTER JOIN classes c
ON s.class_id = c.id;
```

对于这么多种JOIN查询，到底什么使用应该用哪种呢？其实我们用图来表示结果集就一目了然了。

假设查询语句是：

```sql
SELECT ... FROM tableA ??? JOIN tableB ON tableA.column1 = tableB.column2;
```

我们把tableA看作左表，把tableB看成右表，那么INNER JOIN是选出两张表都存在的记录：

![inner-join](https://www.liaoxuefeng.com/files/attachments/1246892164662976/l)

LEFT OUTER JOIN是选出左表存在的记录：

![left-outer-join](https://www.liaoxuefeng.com/files/attachments/1246893588481376/l)

RIGHT OUTER JOIN是选出右表存在的记录：

![right-outer-join](https://www.liaoxuefeng.com/files/attachments/1246893609222688/l)

FULL OUTER JOIN则是选出左右表都存在的记录：

![full-outer-join](https://www.liaoxuefeng.com/files/attachments/1246893632359424/l)

#### 自连接

自连接可以看成内连接的一种，只是连接的表是自身而已。

一张员工表，包含员工姓名和员工所属部门，要找出与 Jim 处在同一部门的所有员工姓名。

子查询版本

```sql
SELECT name
FROM employee
WHERE department = (
      SELECT department
      FROM employee
      WHERE name = "Jim");
```

自连接版本

```sql
SELECT e1.name
FROM employee AS e1 INNER JOIN employee AS e2
ON e1.department = e2.department
      AND e2.name = "Jim";
```

#### 自然连接

自然连接是把同名列通过等值测试连接起来的，同名列可以有多个。

内连接和自然连接的区别：内连接提供连接的列，而自然连接自动连接所有同名列。

```sql
SELECT A.value, B.value
FROM tablea AS A NATURAL JOIN tableb AS B;
```

### 组合查询

使用 UNION 来组合两个查询，如果第一个查询返回 M 行，第二个查询返回 N 行，那么组合查询的结果一般为 M+N 行。

每个查询必须包含相同的列、表达式和聚集函数。

默认会去除相同行，如果需要保留相同行，使用 UNION ALL。

只能包含一个 ORDER BY 子句，并且必须位于语句的最后。

```sql
SELECT col
FROM mytable
WHERE col = 1
UNION
SELECT col
FROM mytable
WHERE col =2;
```

## 修改数据

关系数据库的基本操作就是增删改查，即CRUD：Create、Retrieve、Update、Delete。其中，对于查询，我们已经详细讲述了 `SELECT`语句的详细用法。

而对于增、删、改，对应的SQL语句分别是：

* INSERT：插入新记录；
* UPDATE：更新已有记录；
* DELETE：删除已有记录。

我们将分别讨论这三种修改数据的语句的使用方法。

### INSERT

当我们需要向数据库表中插入一条新记录时，就必须使用 `INSERT`语句。

`INSERT`语句的基本语法是：

> INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);

例如，我们向 `students`表插入一条新记录，先列举出需要插入的字段名称，然后在 `VALUES`子句中依次写出对应字段的值：

```sql
-- 添加一条新记录
INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);
-- 查询并观察结果:
SELECT * FROM students;
```

注意到我们并没有列出 `id`字段，也没有列出 `id`字段对应的值，这是因为 `id`字段是一个自增主键，它的值可以由数据库自己推算出来。此外，如果一个字段有默认值，那么在 `INSERT`语句中也可以不出现。

要注意，字段顺序不必和数据库表的字段顺序一致，但值的顺序必须和字段顺序一致。也就是说，可以写 `INSERT INTO students (score, gender, name, class_id) ...`，但是对应的 `VALUES`就得变成 `(80, 'M', '大牛', 2)`。

还可以一次性添加多条记录，只需要在 `VALUES`子句中指定多个记录值，每个记录是由 `(...)`包含的一组值：

```sql
-- 一次性添加多条新记录
INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);

SELECT * FROM students;
```

将一个表的内容插入到一个新表

```sql
CREATE TABLE newtable AS
SELECT * FROM mytable;
```

### UPDATE

如果要更新数据库表中的记录，我们就必须使用 `UPDATE`语句。

`UPDATE`语句的基本语法是：

> UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;

例如，我们想更新 `students`表 `id=1`的记录的 `name`和 `score`这两个字段，先写出 `UPDATE students SET name='大牛', score=66`，然后在WHERE子句中写出需要更新的行的筛选条件 `id=1`：

```sql
-- 更新id=1的记录
UPDATE students SET name='大牛', score=66 WHERE id=1;
-- 查询并观察结果:
SELECT * FROM students WHERE id=1;
```

注意到 `UPDATE`语句的 `WHERE`条件和 `SELECT`语句的 `WHERE`条件其实是一样的，因此完全可以一次更新多条记录：

```sql
-- 更新id=5,6,7的记录
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
-- 查询并观察结果:
SELECT * FROM students;
```

在 `UPDATE`语句中，更新字段时可以使用表达式。例如，把所有80分以下的同学的成绩加10分：

``sql
-- 更新score<80的记录
UPDATE students SET score=score+10 WHERE score<80;
-- 查询并观察结果:
SELECT * FROM students;

```

其中，`SET score=score+10`就是给当前行的`score`字段的值加上了`10`。

如果`WHERE`条件没有匹配到任何记录，`UPDATE`语句不会报错，也不会有任何记录被更新。例如：

​```sql
-- 更新id=999的记录
UPDATE students SET score=100 WHERE id=999;
-- 查询并观察结果:
SELECT * FROM students;
```

最后，要特别小心的是，`UPDATE`语句可以没有 `WHERE`条件，例如：

> UPDATE students SET score=60;

这时，整个表的所有记录都会被更新。所以，在执行 `UPDATE`语句时要非常小心，最好先用 `SELECT`语句来测试 `WHERE`条件是否筛选出了期望的记录集，然后再用 `UPDATE`更新。

#### MySQL

在使用MySQL这类真正的关系数据库时，`UPDATE`语句会返回更新的行数以及 `WHERE`条件匹配的行数。

例如，更新 `id=1`的记录时：

```sql
mysql> UPDATE students SET name='大宝' WHERE id=1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

MySQL会返回1，可以从打印的结果 `Rows matched: 1 Changed: 1`看到。

当更新 `id=999`的记录时：

```sql
mysql> UPDATE students SET name='大宝' WHERE id=999;
Query OK, 0 rows affected (0.00 sec)
Rows matched: 0  Changed: 0  Warnings: 0
```

MySQL会返回 `0`，可以从打印的结果 `Rows matched: 0 Changed: 0`看到。

### DELETE

如果要删除数据库表中的记录，我们可以使用 `DELETE`语句

`DELETE`语句的基本语法是：

> DELETE FROM <表名> WHERE ...;

例如，我们想删除 `students`表中 `id=1`的记录，就需要这么写：

```sql
-- 删除id=1的记录
DELETE FROM students WHERE id=1;
-- 查询并观察结果:
SELECT * FROM students;
```

注意到 `DELETE`语句的 `WHERE`条件也是用来筛选需要删除的行，因此和 `UPDATE`类似，`DELETE`语句也可以一次删除多条记录：

```sql
-- 删除id=5,6,7的记录
DELETE FROM students WHERE id>=5 AND id<=7;
-- 查询并观察结果:
SELECT * FROM students;
```

如果 `WHERE`条件没有匹配到任何记录，`DELETE`语句不会报错，也不会有任何记录被删除。例如：

```sql
-- 删除id=999的记录
DELETE FROM students WHERE id=999;
-- 查询并观察结果:
SELECT * FROM students;
```

最后，要特别小心的是，和 `UPDATE`类似，不带 `WHERE`条件的 `DELETE`语句会删除整个表的数据：

> DELETE FROM students;

这时，整个表的所有记录都会被删除。所以，在执行 `DELETE`语句时也要非常小心，最好先用 `SELECT`语句来测试 `WHERE`条件是否筛选出了期望的记录集，然后再用 `DELETE`删除。

TRUNCATE TABLE 可以清空表，也就是删除所有行。

```sql
TRUNCATE TABLE mytable;
```

使用更新和删除操作时一定要用 `WHERE` 子句，不然会把整张表的数据都破坏。可以先用 `SELECT`语句进行测试，防止错误删除。

#### MySQL

在使用MySQL这类真正的关系数据库时，`DELETE`语句也会返回删除的行数以及 `WHERE`条件匹配的行数。

例如，分别执行删除 `id=1`和 `id=999`的记录：

```sql
mysql> DELETE FROM students WHERE id=1;
Query OK, 1 row affected (0.01 sec)

mysql> DELETE FROM students WHERE id=999;
Query OK, 0 rows affected (0.01 sec)
```

## 其它操作

### 视图

视图是虚拟的表，本身不包含数据，也就不能对其进行索引操作。

对视图的操作和对普通表的操作一样。

视图具有如下好处：

* 简化复杂的 SQL 操作，比如复杂的连接；
* 只使用实际表的一部分数据；
* 通过只给用户访问视图的权限，保证数据的安全性；
* 更改数据格式和表示。

```sql
CREATE VIEW myview AS
SELECT Concat(col1, col2) AS concat_col, col3*col4 AS compute_col
FROM mytable
WHERE col5 = val;
```

### 存储过程

存储过程可以看成是对一系列 SQL 操作的批处理。

使用存储过程的好处：

* 代码封装，保证了一定的安全性；
* 代码复用；
* 由于是预先编译，因此具有很高的性能。

命令行中创建存储过程需要自定义分隔符，因为命令行是以 ; 为结束符，而存储过程中也包含了分号，因此会错误把这部分分号当成是结束符，造成语法错误。

包含 in、out 和 inout 三种参数。

给变量赋值都需要用 select into 语句。

每次只能给一个变量赋值，不支持集合的操作。

```sql
delimiter //

create procedure myprocedure( out ret int )
    begin
        declare y int;
        select sum(col1)
        from mytable
        into y;
        select y*y into ret;
    end //

delimiter ;
```

```sql
call myprocedure(@ret);
select @ret;
```

### 游标

在存储过程中使用游标可以对一个结果集进行移动遍历。

游标主要用于交互式应用，其中用户需要对数据集中的任意行进行浏览和修改。

使用游标的四个步骤：

1. 声明游标，这个过程没有实际检索出数据；
2. 打开游标；
3. 取出数据；
4. 关闭游标；

```sql
delimiter //
create procedure myprocedure(out ret int)
    begin
        declare done boolean default 0;

        declare mycursor cursor for
        select col1 from mytable;
        # 定义了一个 continue handler，当 sqlstate '02000' 这个条件出现时，会执行 set done = 1
        declare continue handler for sqlstate '02000' set done = 1;

        open mycursor;

        repeat
            fetch mycursor into ret;
            select ret;
        until done end repeat;

        close mycursor;
    end //
 delimiter ;
```

### 触发器

触发器会在某个表执行以下语句时而自动执行：DELETE、INSERT、UPDATE。

触发器必须指定在语句执行之前还是之后自动执行，之前执行使用 BEFORE 关键字，之后执行使用 AFTER 关键字。BEFORE 用于数据验证和净化，AFTER 用于审计跟踪，将修改记录到另外一张表中。

INSERT 触发器包含一个名为 NEW 的虚拟表。

```sql
CREATE TRIGGER mytrigger AFTER INSERT ON mytable
FOR EACH ROW SELECT NEW.col into @result;

SELECT @result; -- 获取结果
```

DELETE 触发器包含一个名为 OLD 的虚拟表，并且是只读的。

UPDATE 触发器包含一个名为 NEW 和一个名为 OLD 的虚拟表，其中 NEW 是可以被修改的，而 OLD 是只读的。

MySQL 不允许在触发器中使用 CALL 语句，也就是不能调用存储过程。

### 事务管理

基本术语：

* 事务（transaction）指一组 SQL 语句；
* 回退（rollback）指撤销指定 SQL 语句的过程；
* 提交（commit）指将未存储的 SQL 语句结果写入数据库表；
* 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），你可以对它发布回退（与回退整个事务处理不同）。

不能回退 SELECT 语句，回退 SELECT 语句也没意义；也不能回退 CREATE 和 DROP 语句。

MySQL 的事务提交默认是隐式提交，每执行一条语句就把这条语句当成一个事务然后进行提交。当出现 START TRANSACTION 语句时，会关闭隐式提交；当 COMMIT 或 ROLLBACK 语句执行后，事务会自动关闭，重新恢复隐式提交。

设置 autocommit 为 0 可以取消自动提交；autocommit 标记是针对每个连接而不是针对服务器的。

如果没有设置保留点，ROLLBACK 会回退到 START TRANSACTION 语句处；如果设置了保留点，并且在 ROLLBACK 中指定该保留点，则会回退到该保留点。

```sql
START TRANSACTION
// ...
SAVEPOINT delete1
// ...
ROLLBACK TO delete1
// ...
COMMIT
```

### 字符集

基本术语：

* 字符集为字母和符号的集合；
* 编码为某个字符集成员的内部表示；
* 校对字符指定如何比较，主要用于排序和分组。

除了给表指定字符集和校对外，也可以给列指定：

```sql
CREATE TABLE mytable
(col VARCHAR(10) CHARACTER SET latin COLLATE latin1_general_ci )
DEFAULT CHARACTER SET hebrew COLLATE hebrew_general_ci;
```

可以在排序、分组时指定校对：

```sql
SELECT *
FROM mytable
ORDER BY col COLLATE latin1_general_ci;
```

### 权限管理

MySQL 的账户信息保存在 mysql 这个数据库中。

```sql
USE mysql;
SELECT user FROM user;
```

**创建账户**

新创建的账户没有任何权限。

> CREATE USER myuser IDENTIFIED BY 'mypassword';

**修改账户名**

> RENAME USER myuser TO newuser;

**删除账户**

> DROP USER myuser;

**查看权限**

> SHOW GRANTS FOR myuser;

**授予权限**

账户用 username@host 的形式定义，username@% 使用的是默认主机名。

> GRANT SELECT, INSERT ON mydatabase.* TO myuser;

**删除权限**

GRANT 和 REVOKE 可在几个层次上控制访问权限：

* 整个服务器，使用 GRANT ALL 和 REVOKE ALL；
* 整个数据库，使用 ON database.*；
* 特定的表，使用 ON database.table；
* 特定的列；
* 特定的存储过程。

> REVOKE SELECT, INSERT ON mydatabase.* FROM myuser;

**更改密码**

必须使用 Password() 函数进行加密。

> SET PASSWROD FOR myuser = Password('new_password');

## MySQL

安装完MySQL后，除了 `MySQL Server`，即真正的MySQL服务器外，还附赠一个 `MySQL Client`程序。`MySQL Client`是一个命令行客户端，可以通过 `MySQL Client`登录MySQL，然后，输入SQL语句并执行。

打开命令提示符，输入命令 `mysql -u root -p`，提示输入口令。填入MySQL的 `root`口令，如果正确，就连上了 `MySQL Server`，同时提示符变为 `mysql>`：

┌────────────────────────────────────────────────────────┐
│Command Prompt                                    - □ x │
├────────────────────────────────────────────────────────┤
│Microsoft Windows [Version 10.0.0]                      │
│(c) 2015 Microsoft Corporation. All rights reserved.    │
│                                                        │
│C:\> mysql -u root -p                                   │
│Enter password: ******                                  │
│                                                        │
│Server version: 5.7                                     │
│Copyright (c) 2000, 2018, ...                           │
│Type 'help;' or '\h' for help.                          │
│                                                        │
│mysql>                                                  │
│                                                        │
└────────────────────────────────────────────────────────┘
输入 `exit`断开与 `MySQL Server`的连接并返回到命令提示符。

{% note info simple %}
MySQL Client的可执行程序是mysql，MySQL Server的可执行程序是mysqld。
MySQL Client和MySQL Server的关系如下：
{% endnote %}

┌──────────────┐  SQL   ┌──────────────┐
│ MySQL Client │───────>│ MySQL Server │
└──────────────┘  TCP   └──────────────┘
在 `MySQL Client`中输入的SQL语句通过TCP连接发送到 `MySQL Server`。默认端口号是 `3306`，即如果发送到本机 `MySQL Server`，地址就是 `127.0.0.1:3306`。

也可以只安装 `MySQL Client`，然后连接到远程 `MySQL Server`。假设远程 `MySQL Server`的IP地址是 `10.0.1.99`，那么就使用 `-h`指定IP或域名：

> mysql -h 10.0.1.99 -u root -p

### 实用SQL语句

在编写SQL时，灵活运用一些技巧，可以大大简化程序逻辑。

#### 插入或替换

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用 `REPLACE`语句，这样就不必先查询，再决定是否先删除再插入：

> REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);

若 `id=1`的记录不存在，`REPLACE`语句将插入新记录，否则，当前 `id=1`的记录将被删除，然后再插入新记录。

#### 插入或更新

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用 `INSERT INTO ... ON DUPLICATE KEY UPDATE ...`语句：

```sql
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
```

若 `id=1`的记录不存在，`INSERT`语句将插入新记录，否则，当前 `id=1`的记录将被更新，更新的字段由 `UPDATE`指定。

#### 插入或忽略

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用 `INSERT IGNORE INTO ...`语句：

> INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);

若 `id=1`的记录不存在，`INSERT`语句将插入新记录，否则，不执行任何操作。

#### 快照

如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合 `CREATE TABLE`和 `SELECT`：

```sql
-- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
```

新创建的表结构和 `SELECT`使用的表结构完全一致。

#### 写入查询结果集

如果查询结果集需要写入到表中，可以结合 `INSERT`和 `SELECT`，将 `SELECT`语句的结果集直接插入到指定表中。

例如，创建一个统计成绩的表 `statistics`，记录各班的平均成绩：

```sql
CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);
```

然后，我们就可以用一条语句写入各班的平均成绩：

> INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;

确保 `INSERT`语句的列和 `SELECT`语句的列能一一对应，就可以在 `statistics`表中直接保存查询的结果：

```bash
> SELECT * FROM statistics;
+----+----------+--------------+
| id | class_id | average      |
+----+----------+--------------+
|  1 |        1 |         86.5 |
|  2 |        2 | 73.666666666 |
|  3 |        3 | 88.333333333 |
+----+----------+--------------+
3 rows in set (0.00 sec)
```

#### 强制使用指定索引

在查询的时候，数据库系统会自动分析查询语句，并选择一个最合适的索引。但是很多时候，数据库系统的查询优化器并不一定总是能使用最优索引。如果我们知道如何选择索引，可以使用 `FORCE INDEX`强制查询使用指定的索引。例如：

> SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id = 1 ORDER BY id DESC;

指定索引的前提是索引 `idx_class_id`必须存在。

## 事务

在执行SQL语句的时候，某些业务要求，一系列操作必须全部执行，而不能仅执行一部分。例如，一个转账操作：

```sql
-- 从id=1的账户给id=2的账户转账100元
-- 第一步：将id=1的A账户余额减去100
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- 第二步：将id=2的B账户余额加上100
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
```

这两条SQL语句必须全部执行，或者，由于某些原因，如果第一条语句成功，第二条语句失败，就必须全部撤销。

这种把多条语句作为一个整体进行操作的功能，被称为数据库事务。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

可见，数据库事务具有 `ACID`这4个特性：

* A：`Atomic`，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
* C：`Consistent`，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100， B账户则必定加上了100；
* I：`Isolation`，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
* D：`Duration`，持久性，即事务完成后，对数据库数据的修改被持久化存储。

对于单条SQL语句，数据库系统自动将其作为一个事务执行，这种事务被称为 `隐式事务`。

要手动把多条SQL语句作为一个事务执行，使用 `BEGIN`开启一个事务，使用 `COMMIT`提交一个事务，这种事务被称为显式事务，例如，把上述的转账操作作为一个显式事务：

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

很显然多条SQL语句要想作为一个事务执行，就必须使用 `显式事务`。

`COMMIT`是指提交事务，即试图把事务内的所有SQL所做的修改永久保存。如果 `COMMIT`语句执行失败了，整个事务也会失败。

有些时候，我们希望主动让事务失败，这时，可以用 `ROLLBACK`回滚事务，整个事务会失败：

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
ROLLBACK;
```

数据库事务是由数据库系统保证的，我们只需要根据业务逻辑使用它就可以。

**隔离级别**

对于两个并发执行的事务，如果涉及到操作同一条记录的时候，可能会发生问题。因为并发操作会带来数据的不一致性，包括脏读、不可重复读、幻读等。数据库系统提供了隔离级别来让我们有针对性地选择事务的隔离级别，避免数据不一致的问题。

SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：

| Isolation Level  | 脏读（Dirty Read） | 不可重复读（Non Repeatable Read） | 幻读（Phantom Read） |
| ---------------- | -------------- | -------------------------- | ---------------- |
| Read Uncommitted | Yes            | Yes                        | Yes              |
| Read Committed   | -              | Yes                        | Yes              |
| Repeatable Read  | -              | -                          | Yes              |
| Serializable     | -              | -                          | -                |

### Read Uncommitted

`Read Uncommitted`是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是 `脏读（Dirty Read）`。

我们来看一个例子。

首先，我们准备好 `students`表的数据，该表仅一行记录：

```bash
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

| 时刻   | 事务A                                      | 事务B                                      |
| ---- | ---------------------------------------- | ---------------------------------------- |
| 1    | SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; | SET TRANSACTIONISOLATION LEVEL READ UNCOMMITTED; |
| 2    | BEGIN;                                   | BEGIN;                                   |
| 3    | UPDATE students SET name = 'Bob' WHERE id = 1; |                                          |
| 4    |                                          | SELECT * FROM students WHERE id = 1;     |
| 5    | ROLLBACK;                                |                                          |
| 6    |                                          | SELECT * FROM students WHERE id = 1;     |
| 7    |                                          | COMMIT;                                  |

当事务A执行完第3步时，它更新了 `id=1`的记录，但并未提交，而事务B在第4步读取到的数据就是未提交的数据。

随后，事务A在第5步进行了回滚，事务B再次读取 `id=1`的记录，发现和上一次读取到的数据不一致，这就是脏读。

可见，在 `Read Uncommitted`隔离级别下，一个事务可能读取到另一个事务更新但未提交的数据，这个数据有可能是脏数据。

### Read Committed

在 `Read Committed`隔离级别下，一个事务可能会遇到 `不可重复读（Non Repeatable Read）`的问题。

不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

我们仍然先准备好 `students`表的数据：

```
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后，分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻   | 事务A                                      | 事务B                                      |
| ---- | ---------------------------------------- | ---------------------------------------- |
| 1    | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; | SET TRANSACTIONISOLATION LEVEL READ COMMITTED; |
| 2    | BEGIN;                                   | BEGIN;                                   |
| 3    |                                          | SELECT * FROM students WHERE id = 1;     |
| 4    | UPDATE students SET name = 'Bob' WHERE id = 1; |                                          |
| 5    | COMMIT;                                  |                                          |
| 6    |                                          | SELECT * FROM students WHERE id = 1;     |
| 7    |                                          | COMMIT;                                  |

当事务B第一次执行第3步的查询时，得到的结果是 `Alice`，随后，由于事务A在第4步更新了这条记录并提交，所以，事务B在第6步再次执行同样的查询时，得到的结果就变成了 `Bob`，因此，在Read Committed隔离级别下，事务不可重复读同一条记录，因为很可能读到的结果不一致。

### Repeatable Read

在 `Repeatable Read`隔离级别下，一个事务可能会遇到 `幻读（Phantom Read）`的问题。

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

我们仍然先准备好 `students`表的数据：

```bash
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后，分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻   | 事务A                                      | 事务B                                      |
| ---- | ---------------------------------------- | ---------------------------------------- |
| 1    | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; | SET TRANSACTIONISOLATION LEVEL READ COMMITTED; |
| 2    | BEGIN;                                   | BEGIN;                                   |
| 3    |                                          | SELECT * FROM students WHERE id = 99;    |
| 4    | INSERT INTO students (id, name) VALUES (99, 'Bob'); |                                          |
| 5    | COMMIT;                                  |                                          |
| 6    |                                          | SELECT * FROM students WHERE id = 99;    |
| 7    |                                          | UPDATE students SET name = 'Alice' WHERE id = 99; |
| 8    |                                          | SELECT * FROM students WHERE id = 99;    |
| 9    |                                          | COMMIT;                                  |

事务B在第3步第一次读取 `id=99`的记录时，读到的记录为空，说明不存在 `id=99`的记录。随后，事务A在第4步插入了一条 `id=99`的记录并提交。事务B在第6步再次读取 `id=99`的记录时，读到的记录仍然为空，但是，事务B在第7步试图更新这条不存在的记录时，竟然成功了，并且，事务B在第8步再次读取 `id=99`的记录时，记录出现了。

可见，幻读就是没有读到的记录，以为不存在，但其实是可以更新成功的，并且，更新成功后，再次读取，就出现了。

### Serializable

`Serializable`是最严格的隔离级别。在 `Serializable`隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。

虽然 `Serializable`隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用 `Serializable`隔离级别。

#### 默认隔离级别

如果没有指定隔离级别，数据库就会使用默认的隔离级别。在 `MySQL`中，如果使用 `InnoDB`，默认的隔离级别是 `Repeatable Read`。

## 其他

### sql中is和=的区别

1.=为比较运算符，同时也是sql中的赋值运算符， 除 text、ntext 或 image 数据类型的表达式外，=可以用于所有其他表达式，更多是一种数值类型上的判断，对于bool类型的判断会有3个结果TRUE、FALSE 和 UNKNOWN，在判断是否为null则会返回UNKNOWN，所以不能用=判断是否为null

2.is 判断表达式是否为bool类型的结果，以及类型是否为空，在判断是否为null应使用is

### 自定义函数

自定义函数与存储过程的区别：

1.输出参数：自定义函数不能拥有输出参数，因为其本身就是输出参数；而存储过程可以包含输出参数。

2.RETURN语句，自定义函数必须有一条RETURN语句；存储过程不能有RETURN语句

3.可以直接在SQL语句中调用自定义函数；存储过程需要使用call来调用。

创建

```sql
create function 函数名([参数列表]) returns 数据类型

begin

    sql语句;

    return 值;

end;
```

示例：

```sql
create function fundemo(num INT) returns int

BEGIN

RETURN num+666;

END;

-- 调用函数

SELECT fundemo(0);
```

输出：666

再来一个demo：定义一个判断网站浏览量等级的函数，<100为C，100到1000为B，大于1000为A

```sql
DROP FUNCTION IF EXISTS rankABC; -- 存在函数时将函数删除

create function rankABC(num INT) returns VARCHAR(10)

BEGIN

DECLARE rank varchar(10); -- declare关键字定义变量

IF num<=100 THEN

set rank = 'C'; -- set关键字用来给变量赋值

ELSEIF (num>100 and num<1000) THEN

set rank = 'B';

ELSE

set rank = 'A';

  END IF;

return rank;

END

-- 调用函数

SELECT *,rankABC(alexa) from websites;
```

![](https://upload-images.jianshu.io/upload_images/18734336-62781520c79f6a3b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1091/format/webp)

### mybatis中的sql语句

#### select

一个select 元素非常简单。例如：

```Xml
<!-- 查询学生，根据id -->  
<select id="getStudent" parameterType="String" resultMap="studentResultMap">  
    SELECT ST.STUDENT_ID,  
               ST.STUDENT_NAME,  
               ST.STUDENT_SEX,  
               ST.STUDENT_BIRTHDAY,  
               ST.CLASS_ID  
          FROM STUDENT_TBL ST  
         WHERE ST.STUDENT_ID = #{studentID}  
</select>  
```

这条语句就叫做‘getStudent，有一个String参数，并返回一个StudentEntity类型的对象。
注意参数的标识是：#{studentID}。#{}占位符对应传入的参数，可以随意写字符串标识符，但最好还是和传入参数对应起来，保持良好的可读性

占位符 # 和 $ 的区别

`#{}` 和 `${}` 都可以替代参数，即占位符

`#{}` 在SQL语句运行的时候显示的是一个一个的 `？ ？ ？`

例如针对根据某个ID的一条删除语句，ID号用 `#{id}` 替代

```xml
<delete id="deleteData" parameterType="int">
   delete from Products where ProductID = #{id}
</delete>
```

输出的SQL日志如下 ：

```sql
Preparing: delete from Products where ProductID = ? 
==> Parameters: 2(Integer)
<==    Updates: 1
[Products] 已经删除第2条记录！
```

`#{ }`在处理接受的字段会当做字符串处理，会把传入的参数自动加上引号`""`，例如这里如果想要删掉 `ID=2` 的记录

当执行函数 deleteData（2）的时候，输出的SQL语句实际上是：

```delete from Products where ProductID = "2"```

而 `${ }` 不会自动添加引号`""` ，就是简单的字符串拼接，容易引发SQL语句注入的安全隐患，所以一般还是推荐使用 `#{ }`.

select 语句属性配置细节：

| 属性            | 描述                                       | 取值    | 默认    |
| ------------- | ---------------------------------------- | ----- | ----- |
| id            | 在这个模式下唯一的标识符，可被其它语句引用                    |       |       |
| parameterType | 传给此语句的参数的完整类名或别名                         |       |       |
| resultType    | 语句返回值类型的整类名或别名。注意，如果是集合，那么这里填写的是集合的项的整类名或别名，而不是集合本身的类名。（resultType 与resultMap 不能并用) |       |       |
| resultMap     | 引用的外部resultMap 名。结果集映射是MyBatis 中最强大的特性。许多复杂的映射都可以轻松解决。（resultType 与resultMap 不能并用） |       |       |
| flushCache    | 如果设为true，则会在每次语句调用的时候就会清空缓存。select 语句默认设为false | true  | false |
| useCache      | 如果设为true，则语句的结果集将被缓存。select 语句默认设为false true | false | false |
| timeout       | 设置驱动器在抛出异常前等待回应的最长时间，默认为不设值，由驱动器自己决定     | true  | false |
| timeout       | 设置驱动器在抛出异常前等待回应的最长时间，默认为不设值，由驱动器自己决定     | 正整数   | 未设置   |
| fetchSize     | 设置一个值后，驱动器会在结果集数目达到此数值后，激发返回，默认为不设值，由驱动器自己决定 | 正整数   | 驱动器决定 |
|statementType|statement，preparedstatement，callablestatement。
预准备语句、可调用语句|STATEMENT
PREPARED
CALLABLE|PREPARED
|resultSetType|forward_only，scroll_sensitive，scroll_insensitive
只转发，滚动敏感，不区分大小写的滚动|FORWARD_ONLY|
SCROLL_SENSITIVE
SCROLL_INSENSITIVE|驱动器决定|

#### insert

一个简单的insert语句：

```Xml
<!-- 插入学生 -->  
<insert id="insertStudent" parameterType="StudentEntity">  
        INSERT INTO STUDENT_TBL (STUDENT_ID,  
                                          STUDENT_NAME,  
                                          STUDENT_SEX,  
                                          STUDENT_BIRTHDAY,  
                                          CLASS_ID)  
              VALUES   (#{studentID},  
                          #{studentName},  
                          #{studentSex},  
                          #{studentBirthday},  
                          #{classEntity.classID})  
</insert>  
```

 insert可以使用数据库支持的自动生成主键策略，设置useGeneratedKeys=”true”，然后把keyProperty 设成对应的列，就搞定了。比如说上面的StudentEntity 使用auto-generated 为id 列生成主键.
 还可以使用selectKey元素。下面例子，使用mysql数据库nextval('student')为自定义函数，用来生成一个key。

```Xml
<!-- 插入学生 自动主键-->  
<insert id="insertStudentAutoKey" parameterType="StudentEntity">  
    <selectKey keyProperty="studentID" resultType="String" order="BEFORE">  
            select nextval('student')  
    </selectKey>  
        INSERT INTO STUDENT_TBL (STUDENT_ID,  
                                 STUDENT_NAME,  
                                 STUDENT_SEX,  
                                 STUDENT_BIRTHDAY,  
                                 CLASS_ID)  
              VALUES   (#{studentID},  
                        #{studentName},  
                        #{studentSex},  
                        #{studentBirthday},  
                        #{classEntity.classID})      
</insert>  
```

insert语句属性配置细节：

| 属性            | 描述                                       | 取值   | 默认    |
| ------------- | ---------------------------------------- | ---- | ----- |
| id            | 在这个模式下唯一的标识符，可被其它语句引用                    |      |       |
| parameterType | 传给此语句的参数的完整类名或别名                         |      |       |
| flushCache    | 如果设为true，则会在每次语句调用的时候就会清空缓存。select 语句默认设为false | true | false |
| useCache      | 如果设为true，则语句的结果集将被缓存。select 语句默认设为false  | true | false |
| timeout       | 设置驱动器在抛出异常前等待回应的最长时间，默认为不设值，由驱动器自己决定     | true | false |
| timeout       | 设置驱动器在抛出异常前等待回应的最长时间，默认为不设值，由驱动器自己决定     | 正整数  | 未设置   |
| fetchSize     | 设置一个值后，驱动器会在结果集数目达到此数值后，激发返回，默认为不设值，由驱动器自己决定 | 正整数  | 驱动器决定 |
statementType, statement，preparedstatement，callablestatement。
预准备语句、可调用语句|STATEMENT
PREPARED
CALLABLE|PREPARED|
|useGeneratedKeys|告诉MyBatis 使用JDBC 的getGeneratedKeys 方法来获取数据库自己生成的主键（MySQL、SQLSERVER 等
关系型数据库会有自动生成的字段）。默认：false|
true|false|false|
|keyProperty|
标识一个将要被MyBatis 设置进getGeneratedKeys 的key 所返回的值，或者为insert 语句使用一个selectKey
子元素。|||

selectKey语句属性配置细节：

| 属性          | 描述                                       | 取值   |
| ----------- | ---------------------------------------- | ---- |
| keyProperty | selectKey 语句生成结果需要设置的属性。                 |      |
| resultType  | 生成结果类型，MyBatis 允许使用基本的数据类型，包括String 、int类型。 |      |
|order|可以设成BEFORE 或者AFTER，如果设为BEFORE，那它会先选择主键，然后设置keyProperty，再执行insert语句；如果设为AFTER，它就先运行insert 语句再运行selectKey 语句，通常是insert 语句中内部调用数据库（像Oracle）内嵌的序列机制。 |BEFORE
AFTER|
|statementType|像上面的那样， MyBatis 支持STATEMENT，PREPARED和CALLABLE 的语句形式， 对应Statement ，PreparedStatement 和CallableStatement 响应|STATEMENT
PREPARED
CALLABLE|

#### update、delete

一个简单的update：

```Xml
<!-- 更新学生信息 -->  
<update id="updateStudent" parameterType="StudentEntity">  
        UPDATE STUDENT_TBL  
            SET STUDENT_TBL.STUDENT_NAME = #{studentName},   
                STUDENT_TBL.STUDENT_SEX = #{studentSex},  
                STUDENT_TBL.STUDENT_BIRTHDAY = #{studentBirthday},  
                STUDENT_TBL.CLASS_ID = #{classEntity.classID}  
         WHERE STUDENT_TBL.STUDENT_ID = #{studentID};     
</update>  
```

一个简单的delete：

```Xml
<!-- 删除学生 -->  
<delete id="deleteStudent" parameterType="StudentEntity">  
        DELETE FROM STUDENT_TBL WHERE STUDENT_ID = #{studentID}  
</delete>  
```

 update、delete语句属性配置细节：

| 属性            | 描述                                       | 取值   | 默认    |
| ------------- | ---------------------------------------- | ---- | ----- |
| id            | 在这个模式下唯一的标识符，可被其它语句引用                    |      |       |
| parameterType | 传给此语句的参数的完整类名或别名                         |      |       |
| flushCache    | 如果设为true，则会在每次语句调用的时候就会清空缓存。select 语句默认设为false | true | false |
| useCache      | 如果设为true，则语句的结果集将被缓存。select 语句默认设为false  | true | false |
| timeout       | 设置驱动器在抛出异常前等待回应的最长时间，默认为不设值，由驱动器自己决定     | true | false |
| timeout       | 设置驱动器在抛出异常前等待回应的最长时间，默认为不设值，由驱动器自己决定     | 正整数  | 未设置   |
| fetchSize     | 设置一个值后，驱动器会在结果集数目达到此数值后，激发返回，默认为不设值，由驱动器自己决定 | 正整数  | 驱动器决定 |
statementType, statement，preparedstatement，callablestatement。
预准备语句、可调用语句|STATEMENT
PREPARED
CALLABLE|PREPARED|

#### sql

Sql元素用来定义一个可以复用的SQL 语句段，供其它语句调用。比如：

```Xml
<!-- 复用sql语句  查询student表所有字段 -->  
<sql id="selectStudentAll">  
        SELECT ST.STUDENT_ID,  
                   ST.STUDENT_NAME,  
                   ST.STUDENT_SEX,  
                   ST.STUDENT_BIRTHDAY,  
                   ST.CLASS_ID  
              FROM STUDENT_TBL ST  
</sql>  
```

   这样，在select的语句中就可以直接引用使用了，将上面select语句改成：

```Xml
<!-- 查询学生，根据id -->  
<select id="getStudent" parameterType="String" resultMap="studentResultMap">  
    <include refid="selectStudentAll"/>  
            WHERE ST.STUDENT_ID = #{studentID}   
</select>  
```

#### parameters

上面很多地方已经用到了参数，比如查询、修改、删除的条件，插入，修改的数据等，MyBatis可以使用的基本数据类型和Java的复杂数据类型。

基本数据类型，String，int，date等。

但是使用基本数据类型，只能提供一个参数，所以需要使用Java实体类，或Map类型做参数类型。通过#{}可以直接得到其属性。

**基本类型参数**

根据入学时间，检索学生列表：

```Xml
<!-- 查询学生list，根据入学时间  -->  
<select id="getStudentListByDate"  parameterType="Date" resultMap="studentResultMap">  
    SELECT *  
      FROM STUDENT_TBL ST LEFT JOIN CLASS_TBL CT ON ST.CLASS_ID = CT.CLASS_ID  
     WHERE CT.CLASS_YEAR = #{classYear};      
</select>  
```

```Java
List<StudentEntity> studentList = studentMapper.getStudentListByClassYear(StringUtil.parse("2007-9-1"));  
for (StudentEntity entityTemp : studentList) {  
    System.out.println(entityTemp.toString());  
}  
```

**Java实体类型参数**

 根据姓名和性别，检索学生列表。使用实体类做参数：

```Xml
<!-- 查询学生list，like姓名、=性别，参数entity类型 -->  
<select id="getStudentListWhereEntity" parameterType="StudentEntity" resultMap="studentResultMap">  
    SELECT * from STUDENT_TBL ST  
        WHERE ST.STUDENT_NAME LIKE CONCAT(CONCAT('%', #{studentName}),'%')  
          AND ST.STUDENT_SEX = #{studentSex}  
</select>  
```

```Java
StudentEntity entity = new StudentEntity();  
entity.setStudentName("李");  
entity.setStudentSex("男");  
List<StudentEntity> studentList = studentMapper.getStudentListWhereEntity(entity);  
for (StudentEntity entityTemp : studentList) {  
    System.out.println(entityTemp.toString());  
}  
```

**Map参数**

根据姓名和性别，检索学生列表。使用Map做参数：

```Xml
<!-- 查询学生list，=性别，参数map类型 -->  
<select id="getStudentListWhereMap" parameterType="Map" resultMap="studentResultMap">  
    SELECT * from STUDENT_TBL ST  
     WHERE ST.STUDENT_SEX = #{sex}  
          AND ST.STUDENT_SEX = #{sex}  
</select>  
```

```Java
Map<String, String> map = new HashMap<String, String>();  
map.put("sex", "女");  
map.put("name", "李");  
List<StudentEntity> studentList = studentMapper.getStudentListWhereMap(map);  
for (StudentEntity entityTemp : studentList) {  
    System.out.println(entityTemp.toString());  
}  
```

**多参数的实现**

如果想传入多个参数，则需要在接口的参数上添加@Param注解。给出一个实例：

接口写法：

```Java
public List<StudentEntity> getStudentListWhereParam(@Param(value = "name") String name, @Param(value = "sex") String sex, @Param(value = "birthday") Date birthdar, @Param(value = "classEntity") ClassEntity classEntity);  
```

SQL写法：

```Xml
<!-- 查询学生list，like姓名、=性别、=生日、=班级，多参数方式 -->  
<select id="getStudentListWhereParam" resultMap="studentResultMap">  
    SELECT * from STUDENT_TBL ST  
    <where>  
        <if test="name!=null and name!='' ">  
            ST.STUDENT_NAME LIKE CONCAT(CONCAT('%', #{name}),'%')  
        </if>  
        <if test="sex!= null and sex!= '' ">  
            AND ST.STUDENT_SEX = #{sex}  
        </if>  
        <if test="birthday!=null">  
            AND ST.STUDENT_BIRTHDAY = #{birthday}  
        </if>  
        <if test="classEntity!=null and classEntity.classID !=null and classEntity.classID!='' ">  
            AND ST.CLASS_ID = #{classEntity.classID}  
        </if>  
    </where>  
</select>  
```

进行查询：

```Java
List<StudentEntity> studentList = studentMapper.getStudentListWhereParam("", "",StringUtil.parse("1985-05-28"), classMapper.getClassByID("20000002"));  
for (StudentEntity entityTemp : studentList) {  
    System.out.println(entityTemp.toString());  
}  
```

**字符串代入法**

默认的情况下，使用#{}语法会促使MyBatis 生成PreparedStatement 属性并且使用PreparedStatement 的参数（=？）来安全的设置值。尽量这些是快捷安全，也是经常使用的。但有时候你可能想直接未更改的字符串代入到SQL 语句中。比如说，对于ORDER BY，你可能会这样使用：ORDER BY ${columnName}但MyBatis 不会修改和规避掉这个字符串。

注意：这样地接收和应用一个用户输入到未更改的语句中，是非常不安全的。这会让用户能植入破坏代码，所以，要么要求字段不要允许客户输入，要么你直接来检测他的合法性 。

#### cache缓存

MyBatis 包含一个强在的、可配置、可定制的缓存机制。MyBatis 3 的缓存实现有了许多改进，既强劲也更容易配置。默认的情况，缓存是没有开启，除了会话缓存以外，它可以提高性能，且能解决全局依赖。开启二级缓存，你只需要在SQL 映射文件中加入简单的一行：

> <cache/>

这句简单的语句的作用如下：

1. 所有在映射文件里的select 语句都将被缓存。
2. 所有在映射文件里insert,update 和delete 语句会清空缓存。
3. 缓存使用“最近很少使用”算法来回收
4. 缓存不会被设定的时间所清空。
5. 每个缓存可以存储1024 个列表或对象的引用（不管查询出来的结果是什么）。
6. 缓存将作为“读/写”缓存，意味着获取的对象不是共享的且对调用者是安全的。不会有其它的调用
7. 者或线程潜在修改。

例如，创建一个FIFO 缓存让60 秒就清空一次，存储512 个对象结果或列表引用，并且返回的结果是只读。因为在不用的线程里的两个调用者修改它们可能会导致引用冲突。

```Xml
<cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true">  
</cache>  
```

还可以在不同的命名空间里共享同一个缓存配置或者实例。在这种情况下，你就可以使用cache-ref 来引用另外一个缓存。

```Xml
<cache-ref namespace="com.liming.manager.data.StudentMapper"/>  
```

Cache 语句属性配置细节：

|属性|说明|取值|默认值|
|----|----|---|------|
|eviction|缓存策略：
LRU - 最近最少使用法：移出最近较长周期内都没有被使用的对象。
FIFI- 先进先出：移出队列里较早的对象
SOFT - 软引用：基于软引用规则，使用垃圾回收机制来移出对象
WEAK - 弱引用：基于弱引用规则，使用垃圾回收机制来强制性地移出对象|LRU
FIFI
SOFT
WEAK|LRU|
|flushInterval|代表一个合理的毫秒总计时间。默认是不设置，因此使用无间隔清空即只能调用语句来清空。|正整数|不设置|
|size|缓存的对象的大小|正整数|1024|
|readOnly|
只读缓存将对所有调用者返回同一个实例。因此都不能被修改，这可以极大的提高性能。可写的缓存将通过序列化来返回一个缓存对象的拷贝。这会比较慢，但是比较安全。所以默认值是false。| true|false |false|

### mybatis中的sql标签

#### if

if标签通常用于where语句中，通过判断参数值来决定是否使用某个查询条件，它也经常用于UPDATE语句中判断是否更新某一个字段，还可以再INSERT语句中用来判断是否插入某个字段的值。

##### 在where条件中使用if

用法：含义就是当if满足时，就执行标签体中的内容。

需求：准备如下数据表，当只输入用户名的时候，就根据这个用户名进行模糊查询匹配的用户；当只输入邮箱时，就根据邮箱完全匹配用户胡；当两者都同时输入时，就需要用这两个条件去查询匹配的用户。

表格：

![](https://img2020.cnblogs.com/blog/1435948/202003/1435948-20200329155126983-923390020.png)

贴下关键代码：

 （1）controller

```java
@PostMapping("/selectByUser")
    public List<SysUser> selectByUser(){
        SysUser sysUser=new SysUser();
        sysUser.setUserName("tes");
        return userService.selectByUser(sysUser);
    }
```

说明：这里只传入了userName。而userEmail为null

（2）service实现类

```java
1  @Override
2     public List<SysUser> selectByUser(SysUser sysUser) {
3         return userMapper.selectByUser(sysUser);
4     }
```

（3）mapper接口

```List<SysUser> selectByUser(SysUser sysUser);```

（4）mapper.xml

```xml
    <select id="selectByUser" resultType="com.example.demo.dao.SysUser">`       
        select id,        
        user_name userName,
        user_password userPassword,
        user_email userEmail,
        user_info userInfo,
        head_img headImg,
        created_time createTime
        from sys_user
        where 1=1
        <if test="userName!=null and userName!=''">
            and user_name like concat('%',#{userName},'%')
        </if>
        <if test="userEmail!=null and userEmail!=''">
            and user_email=#{userEmail}
        </if>
    </select>
```

说明：1.if标签内有一个必填的属性test,test的值是一个符合OGNL要求的判断表达式，表达式的结果可以认为是ture或者false.
     2.当有多个判断条件时，可以使用and或者or进行搭配。
     3.上面两个if，它们是平级的关系，只要有满足if条件的,if标签内的 and user_x=... 这一句就会接在where语句后面。如果不满足，就不会有。
     4.这里写了一个where 1=1 是为了保证当2个if语句都没有的时候，不会直接以where结束，这样sql语句就会报错。
       这种写法where1=1不美观，后面可以采用where标签来拯救它。

（5）实体类

```java
 1 package com.example.demo.dao;
 2 
 3 
 4 import java.sql.Date;
 5 
 6 public class SysUser {
 7     /**
 8      * 用户ID
 9      */
10     private Long id;
11     /**
12      * 用户名
13      */
14     private String userName;
15     /**
16      * 密码
17      */
18     private String userPassword;
19     /**
20      * 邮箱
21      */
22     private String userEmail;
23     /**
24      * 简介
25      */
26     private String userInfo;
27     /**
28      * 头像
29      */
30     private byte[] headImg;
31     /**
32      * 创建时间
33      */
34     private Date createTime;
35 
36     public Long getId() {
37         return id;
38     }
39 
40     public void setId(Long id) {
41         this.id = id;
42     }
43 
44     public String getUserName() {
45         return userName;
46     }
47 
48     public void setUserName(String userName) {
49         this.userName = userName;
50     }
51 
52     public String getUserPassword() {
53         return userPassword;
54     }
55 
56     public void setUserPassword(String userPassword) {
57         this.userPassword = userPassword;
58     }
59 
60     public String getUserEmail() {
61         return userEmail;
62     }
63 
64     public void setUserEmail(String userEmail) {
65         this.userEmail = userEmail;
66     }
67 
68     public String getUserInfo() {
69         return userInfo;
70     }
71 
72     public void setUserInfo(String userInfo) {
73         this.userInfo = userInfo;
74     }
75 
76     public byte[] getHeadImg() {
77         return headImg;
78     }
79 
80     public void setHeadImg(byte[] headImg) {
81         this.headImg = headImg;
82     }
83 
84     public Date getCreateTime() {
85         return createTime;
86     }
87 
88     public void setCreateTime(Date createTime) {
89         this.createTime = createTime;
90     }
91 }
```

采用postman模拟请求，会得到三条和名字模糊匹配的记录：

```
 1 [
 2     {
 3         "id": 3,
 4         "userName": "test1",
 5         "userPassword": "123456",
 6         "userEmail": "test1@mybatis.tk",
 7         "userInfo": "test1",
 8         "headImg": null,
 9         "createTime": "2020-03-04"
10     },
11     {
12         "id": 1001,
13         "userName": "test",
14         "userPassword": "123456",
15         "userEmail": "test@mybatis.tk",
16         "userInfo": "管理员",
17         "headImg": null,
18         "createTime": "2020-03-19"
19     },
20     {
21         "id": 1002,
22         "userName": "test1",
23         "userPassword": "123456",
24         "userEmail": "test1@mybatis.tk",
25         "userInfo": "test1",
26         "headImg": null,
27         "createTime": "2020-03-25"
28     },
29     {
30         "id": 1003,
31         "userName": "test1",
32         "userPassword": "123456",
33         "userEmail": "test1@mybatis.tk",
34         "userInfo": "test1",
35         "headImg": null,
36         "createTime": "2020-03-29"
37     },
38     {
39         "id": 1004,
40         "userName": "test1",
41         "userPassword": "123456",
42         "userEmail": "test1@mybatis.tk",
43         "userInfo": "test1",
44         "headImg": null,
45         "createTime": "2020-03-29"
46     }
47 ]
```

##### 在UPDATE更新列中使用if标签

需求：只更新发生变化的字段。

（1）controller

```java
1  @PostMapping("/updateBySelective")
2     int updateBySelective(SysUser sysUser){
3         sysUser.setUserName("hello");
4         sysUser.setId(1L);
5         return userService.updateBySelective(sysUser);
6     }
```

（2）service实现类

```java
1 @Override
2     public int updateBySelective(SysUser sysUser) {
3         return userMapper.updateBySelective(sysUser);
4     }
```

（3）mapper接口

```java
1  int updateBySelective(SysUser sysUser);
```

（4）mapper.xml

```xml
<update id="updateBySelective">
        update sys_user
        set
        <if test="userName!=null and userName!=''">
            user_name=#{userName},
        </if>
        <if test="userPassword!=null and userPassword!=''">
            user_password=#{userPassword},
        </if>
        <if test="userEmail!=null and userEmail!=''">
            user_email=#{userEmail},
        </if>
        <if test="userInfo!=null and userInfo!=''">
            user_info=#{userInfo},
        </if>
        <if test="headImg!=null">
            head_img=#{headImg,jdbcType=BLOB},
        </if>
        <if test="createTime!=null">
            created_time=#{createTime,jdbcType=TIMESTAMP},
        </if>
        id=#{id}
        where id=#{id}
    </update>
```

说明：1.这里 id=#{id}是为了防止所有if都不满足的时候语法错误。即使if中有个别满足的，也不能省去，否则sql语法错误。
       这种写法不美观，仍然然可以通过where标签和set标签来解决。

![](https://img2020.cnblogs.com/blog/1435948/202003/1435948-20200329162456705-455078904.png)

##### 在INSERT动态插入列中使用if标签

需求：如果某一列的参数值不为空，就使用传入的值；如果传入参数为空，就使用数据库中默认的值（通常为空），而不使用传入的空值。

（1）controller

```java
1 @PostMapping("/insertUser")
2     public int insertUser(){
3 
4        SysUser sysUser=new SysUser();
5        sysUser.setUserName("Tom");
6        sysUser.setUserEmail("aa.@qq.com");
7        sysUser.setUserInfo("作家");
8        return userService.insertUser(sysUser);
9     }
```

（2）service实现类

```java
1 @Override
2     public int insertUser(SysUser sysUser) {
3         return userMapper.insertUser(sysUser);
4     }
```

（3）mapper接口

```int insertUser(SysUser sysUser);```

（4）mapper.xml

```xml
<insert id="insertUser">
    insert into sys_user(
    user_name,user_password,user_email,
    <if test="userInfo!=null and userInfo!=''">
        user_info,
    </if>
    head_img,created_time)
    values(
    #{userName},#{userPassword},#{userEmail},
    <if test="userInfo!=null and userInfo!=''">
        #{userInfo},
    </if>
    #{headImg,jdbcType=BLOB},#{createTime,jdbcType=TIMESTAMP})
</insert>
```

#### choose(when、otherwise）

if标签只能实现if功能，没有else功能。于是可以通过本节的组合来实现if..else的功能。choose中农包含when和otherwise两个标签，一个choose中至少有一个when，有0个或1个otherwise.  

我的理解：when就相当于java中的else if，而otherwise就相当于java中的else。

需求：准备如下表。当ID有值的时候就优先采用ID进行查询匹配用户；如果ID没有值，那就采用用户名进行查询匹配的用户。

![](https://img2020.cnblogs.com/blog/1435948/202003/1435948-20200329164526213-1738106582.png)

（1）controller

```java
1 @PostMapping("/selectByIdOrName")
2     SysUser selectByIdOrName(){
3         SysUser sysUser=new SysUser();
4         sysUser.setId(1L);
5         sysUser.setUserName("test3");
6         return userService.selectByIdOrName(sysUser);
7     }
```

（2）service实现类

```java
1 @Override
2     public SysUser selectByIdOrName(SysUser sysUser) {
3         return userMapper.selectByIdOrName(sysUser);
4     }
```

（3）mapper接口

```SysUser selectByIdOrName(SysUser sysUser);```

（4）mapper.xml

```xml
<select id="selectByIdOrName" resultType="com.example.demo.dao.SysUser">
    select id,
    user_name userName,
    user_password userPassword,
    user_email userEmail,
    user_info userInfo,
    head_img headImg,
    created_time createTime
    from sys_user
    where 1=1
    <choose>
        <when test="id!=null">
            and id=#{id}
        </when>
        <when test="userName!=null and userName!=''">
            and user_name=#{userName}
        </when>
        <otherwise>
            and 1=2
        </otherwise>
    </choose>
</select>
```

postman

```
{
    "id": 1,
    "userName": "test1",
    "userPassword": "123456",
    "userEmail": "admin@mybatis.tk",
    "userInfo": "管理员",
    "headImg": null,
    "createTime": "2020-03-01"
}
```

#### where、set、trim

##### where

修改之前的where 1=1例子，姓名模糊匹配，邮箱完全匹配查询。

修改之前：

```xml
<select id="selectByUser" resultType="com.example.demo.dao.SysUser">
        select id,
        user_name userName,
        user_password userPassword,
        user_email userEmail,
        user_info userInfo,
        head_img headImg,
        created_time createTime
        from sys_user
        where 1=1
        <if test="userName!=null and userName!=''">
            and user_name like concat('%',#{userName},'%')
        </if>
        <if test="userEmail!=null and userEmail!=''">
            and user_email=#{userEmail}
        </if>
    </select>
```

直接将xml文件改为如下：

```xml
<select id="selectByUser" resultType="com.example.demo.dao.SysUser">
        select id,
        user_name userName,
        user_password userPassword,
        user_email userEmail,
        user_info userInfo,
        head_img headImg,
        created_time createTime
        from sys_user
        <where>
            <if test="userName!=null and userName!=''">
                and user_name like concat('%',#{userName},'%')
            </if>
            <if test="userEmail!=null and userEmail!=''">
                and user_email=#{userEmail}
            </if>
        </where>

    </select>
```

说明：当if都不满足的时候，where中就没有元素，就不会存在之前缺少where 1=1造成sql错误的问题。

##### set用法

与where标签类似的，在update语句里也会碰到多个字段相关的问题。 在这种情况下，就可以使用set标签。
其效果与where标签类似，有数据的时候才进行设置。

还是之前的例子：

修改之前：

```xml
<update id="updateBySelective">
        update sys_user
        set
        <if test="userName!=null and userName!=''">
            user_name=#{userName},
        </if>
        <if test="userPassword!=null and userPassword!=''">
            user_password=#{userPassword},
        </if>
        <if test="userEmail!=null and userEmail!=''">
            user_email=#{userEmail},
        </if>
        <if test="userInfo!=null and userInfo!=''">
            user_info=#{userInfo},
        </if>
        <if test="headImg!=null">
            head_img=#{headImg,jdbcType=BLOB},
        </if>
        <if test="createTime!=null">
            created_time=#{createTime,jdbcType=TIMESTAMP},
        </if>
        id=#{id}
        where id=#{id}
    </update>
```

修改之后：

```xml
 1 <update id="updateUser">
 2         update sys_user
 3         set
 4         <foreach collection="_parameter" item="val" index="key" separator=",">
 5             ${key}=#{val}
 6         </foreach>
 7         where id=#{id}
 8     </update>
 9 
10     <update id="updateBySelective">
11         update sys_user
12         <set>
13             <if test="userName!=null and userName!=''">
14                 user_name=#{userName},
15             </if>
16             <if test="userPassword!=null and userPassword!=''">
17                 user_password=#{userPassword},
18             </if>
19             <if test="userEmail!=null and userEmail!=''">
20                 user_email=#{userEmail},
21             </if>
22             <if test="userInfo!=null and userInfo!=''">
23                 user_info=#{userInfo},
24             </if>
25             <if test="headImg!=null">
26                 head_img=#{headImg,jdbcType=BLOB},
27             </if>
28             <if test="createTime!=null">
29                 created_time=#{createTime,jdbcType=TIMESTAMP},
30             </if>
31             id=#{id}
32         </set>
33         where id=#{id}
34     </update>
```

这种只解决了逗号问题，不是很全面的解决问题，仍然需要

id=#{id}

##### trim标签

where和set标签的功能都能通过trim标签实现。

![](https://img2020.cnblogs.com/blog/1435948/202003/1435948-20200329171906958-413431918.png)

 举例：

（1）还是之前的用户名模糊匹配，邮箱完全匹配的例子：（使用trim标签去除多余的and关键字）

修改mapper.xml文件：

```xml
<select id="selectByUser" resultType="com.example.demo.dao.SysUser">
        select id,
        user_name userName,
        user_password userPassword,
        user_email userEmail,
        user_info userInfo,
        head_img headImg,
        created_time createTime
        from sys_user
        <trim prefix="where" prefixOverrides="and">
            <if test="userName!=null and userName!=''">
                and user_name like concat('%',#{userName},'%')
            </if>
            <if test="userEmail!=null and userEmail!=''">
                and user_email=#{userEmail}
            </if>
        </trim>

    </select>
```

效果一样的。但是就不会多写where1=1，并且指定了prefix为where。

#### foreach标签

作用是：
foreach标签通常用于in 这样的语法里。

用法（接收一个List集合参数）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.myjava.pojo">
    <select id="listProduct" resultType="Product">
          SELECT * FROM product_
            WHERE ID in
                <foreach item="item" index="index" collection="list"
                    open="(" separator="," close=")">
                    #{item}
                </foreach>
    </select>
    </mapper>
```

#### bind标签

bind标签就像是再做一次字符串拼接，网上也有说叫绑定，差不多意思，只是方便后续的使用。

用法：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.myjava.pojo">
 
        <select id="listProduct" resultType="Product">
            <bind name="likename" value="'%' + name + '%'" />
            select * from   product_  where name like #{likename}
        </select>
 
    </mapper>
```

#### sql片段标签

作用是：

通过该标签可定义能复用的sql语句片段，在执行sql语句标签中直接引用即可。
这样既可以提高编码效率，还能有效简化代码，提高可读性。

用法：

```xml
<!--定义sql片段-->
<sql id="orderAndItem">
    o.order_id,o.cid,o.address,o.create_date,o.orderitem_id,i.orderitem_id,i.product_id,i.count
</sql>
<select id="findOrderAndItemsByOid" parameterType="java.lang.String" resultMap="BaseResultMap">
    select
<!--引用sql片段-->
    <include refid="orderAndItem" />
    from ordertable o
    join orderitem i on o.orderitem_id = i.orderitem_id
    where o.order_id = #{orderId}
</select>
```

### case语句的使用

CASE是一个控制流语句，其作用与IF-THEN-ELSE语句非常相似，可根据数据选择值。 CASE语句遍历条件并在满足第一个条件时返回值。 因此，一旦条件成立，它将短路，从而忽略后面的子句并返回结果。 正如我们在今天的博客中看到的那样，它可以用来测试条件和离散值。

基本语法
CASE语句有两种形式：第一种评估一个或多个条件，并返回第一个符合条件的结果。 如果没有条件是符合的，则返回ELSE子句部分的结果，如果没有ELSE部分，则返回NULL：

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```

第二种CASE句法返回第一个`value = compare_value`比较结果为真的结果。 如果没有比较结果符合，则返回ELSE后的结果，如果没有ELSE部分，则返回NULL：

```sql
CASE compare_value
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```
