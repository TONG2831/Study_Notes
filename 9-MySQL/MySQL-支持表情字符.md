## MySQL-存储表情字符😀

#### 1.emoji

> emoji 是一种特殊的 Unicode 编码



#### 2.utf8

> utf8是unicode的实现方式之一
>
> 在`mysql`中`utf8`是`utf8mb3`的别称
>
> utf8中,一个字符使用1-3个字节表示,中文3个字节

#### 3.utf8mb4

> 同utf8一样,也是unicode的实现方式之一
>
> 不同于`utf8`,`utf8mb4`使用1-4字节表示字符
>
> 表情字符是4个字节,所以,`mysql`中需要使用该编码方式

#### 4.问题产生:数据库和表的编码方式都是`utf8mb4`,但是插入的数据显示全是`????`

##### 4.1创建的数据库和创建的表的编码都是`utf8mb4`,但是通过代码插入的数据显示都是`???`.

```java
 Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/test?useUnicode=true&amp;characterEncoding=utf8mb4", "root", "root");
            PreparedStatement preparedStatement = connection.prepareStatement("insert into chat_log(user_id,emoji) " +
                    "value(1,?)");
            preparedStatement.setString(1,"😀😀😀");

            preparedStatement.execute();
```

##### 4.2复制,直接黏贴表情符到数据库中,显示正确(mysql工具,navicate pre)

##### 4.3问题原因

> 问题还是处在mysql的字符集上

使用命令`show variables where variable_name like 'character_set_%';` (我已经修改过)

```sql
+--------------------------+-----------------------------------------------------------+
| Variable_name            | Value                                                     |
+--------------------------+-----------------------------------------------------------+
| character_set_client     | utf8mb4                                                   |
| character_set_connection | utf8mb4                                                   |
| character_set_database   | utf8mb4                                                   |
| character_set_filesystem | binary                                                    |
| character_set_results    | utf8mb4                                                   |
| character_set_server     | utf8mb4                                                   |
| character_set_system     | utf8                                                      |
| character_sets_dir       | /usr/local/mysql-5.7.25-macos10.14-x86_64/share/charsets/ |
+--------------------------+-----------------------------------------------------------+
8 rows in set (0.00 sec)

```

应当保证此处的编码方式如上图.

##### 4.5 如何设置编码方式

- 通过命令行:set character_set_client=utf8mb4,只是临时修改
- 通过修改my.cnf(mac环境,win应该是:my.ini)文件,永久修改

**修改my.cnf文件**

mac命令

```
sudo cp /usr/local/mysql/support-files/my-medium.cnf /etc/my.cnf

sudo vi /etc/my.cnf
```

在该文件中加入

```sql
[client]
default-character-set = utf8mb4
[mysql]
default-character-set = utf8mb4
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'
```

保存退出

此时再查看字符集,就已经和上面的相同了

修改完成.