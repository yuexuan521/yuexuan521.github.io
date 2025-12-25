---
title: "网络安全 DVWA通关指南 Cross Site Request Forgery (CSRF)"
date: 2024-09-26 12:24:47
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "csrf"
- "网络安全"
- "安全"
- "DVWA"
- "靶场"
- "DVWA通关指南"
---



## DVWA Cross Site Request Forgery (CSRF)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959091.jpeg)



---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)

---

> CSRF是跨站请求伪造攻击，由客户端发起，是由于没有在执行关键操作时，进行 `是否由用户自愿发起的` 确认攻击者通过用户的浏览器来注入额外的网络请求，来破坏一个网站会话的完整性。
> 
> 比如某网站 **用户信息修改** 功能，没有验证Referer也没添加Token，攻击者可以用HTML构造恶意代码提交POST请求，诱骗已经登陆的受害者点击，可以直接修改用户信息
> 
> **修复建议** 
> 
> > 
> > 
> > - 验证Referer
> > 
> > - 添加token
> > 
> > 
> 
> 

### DVWA Low 级别 CSRF

0、分析网页源代码（路径：“D:\phpstudy_pro\DVWA-master\vulnerabilities\csrf\source\low.php”）

```php
<?php

if(isset($_GET['Change'])) { // 检查是否有请求更改密码的动作
    // 获取用户输入的新密码和确认密码
    $pass_new  = $_GET['password_new'];
    $pass_conf = $_GET['password_conf'];

    // 检查两次输入的密码是否匹配
    if ($pass_new == $pass_conf) { 
        // 密码匹配
        // 防止SQL注入，转义新密码字符串
        $pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"], $pass_new) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
        
        // 对新密码进行MD5哈希加密（注意：MD5加密在此处已经过时，不建议用于存储密码）
        $pass_new = md5($pass_new);

        // 构造SQL更新语句，更新当前登录用户（由dvwaCurrentUser()函数获取）的密码
        $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";

        // 执行SQL查询
        $result = mysqli_query($GLOBALS["___mysqli_ston"], $insert) or die('<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>');

        // 如果密码成功更改，则反馈给用户
        $html .= "<pre>Password Changed.</pre>";
    } else {
        // 如果两次输入的密码不匹配，则反馈错误信息
        $html .= "<pre>Passwords did not match.</pre>";
    }

    // 关闭数据库连接
    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>

```

1、选择DVWA的CSRF，修改密码为111，提交后观察到网站链接发生变化

```
http://dvwa/vulnerabilities/csrf/?password_new=111&password_conf=111&Change=Change#
```

观察链接，认为使用get方式提交修改密码参数，只要三个参数符合就可以执行密码修改的操作

 ![image-20240415193519864](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959092.png)

2、打开一个新标签页，在地址栏输入 `http://dvwa/vulnerabilities/csrf/?password_new=111&password_conf=111&Change=Change#` ，回车后，进入DVWA中并提示Password Changed，修改密码成功。

 ![image-20240415194631245](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959093.png)

 ![image-20240415194532633](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959095.png)

3、可以通过将长连接转换为短链接的方法，诱使用户点击链接，通过 [在线工具](https://uutool.cn/dwz/) 转换

 ![image-20240415194928196](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959096.png)

### DVWA Medium 级别 CSRF

0、分析网页源代码（路径：“D:\phpstudy_pro\DVWA-master\vulnerabilities\csrf\source\medium.php”）

```php
<?php

if( isset( $_GET[ 'Change' ] ) ) {
	// Checks to see where the request came from
    //检查$_SERVER['HTTP_REFERER']，看看请求是否来自包含当前服务器名称$_SERVER['SERVER_NAME']的地址。stripos函数用于查找HTTP_REFERER是否包含服务器名
	if( stripos( $_SERVER[ 'HTTP_REFERER' ] ,$_SERVER[ 'SERVER_NAME' ]) !== false ) {
		// Get input
		$pass_new  = $_GET[ 'password_new' ];
		$pass_conf = $_GET[ 'password_conf' ];

		// Do the passwords match?
		if( $pass_new == $pass_conf ) {
			// They do!
			$pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
			$pass_new = md5( $pass_new );

			// Update the database
			$insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";
			$result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

			// Feedback for the user
			$html .= "<pre>Password Changed.</pre>";
		}
		else {
			// Issue with passwords matching
			$html .= "<pre>Passwords did not match.</pre>";
		}
	}
	else {
		// Didn't come from a trusted source
		$html .= "<pre>That request didn't look correct.</pre>";
	}

	((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>

```

1、尝试修改密码为222，修改成功，得到链接

```
http://dvwa/vulnerabilities/csrf/?password_new=222&password_conf=222&Change=Change#
```

2、打开新的标签页，使用上面的地址，出现错误提示，密码修改错误

 ![image-20240417080206278](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959097.png)

3、打开计算机代理，修改电脑代理服务器IP设置为127.0.0.1，端口设置为8888，BurpSuite调整代理参数与电脑代理一致

 ![image-20240412094734133](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959098.png)

 ![image-20240412095237028](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959099.png)

 ![image-20240412094536046](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959100.png)

4、正常修改密码的网页，使用BurpSuitePro捕获流量包，发现多出Referer属性信息

 ![image-20240418165344132](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959101.png)

5、而直接粘贴链接修改密码失败的报文缺少Referer属性信息，右键选择Send to Repeater

 ![image-20240418170054026](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959102.png)

 ![image-20240418170418876](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959103.png)

5、打开Repeater选项卡，给Request请求中添加Referer信息，Referer需要包括"dvwa"字段（需要符合同源策略），点击Send发送，修改密码成功

 ![image-20240418172052977](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959104.png)

 ![image-20240418172120333](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959105.png)

### DVWA High 级别 CSRF

0、分析网页源代码（路径：“D:\phpstudy_pro\DVWA-master\vulnerabilities\csrf\source\high.php”）

```php
<?php

if( isset( $_GET[ 'Change' ] ) ) {
	// Check Anti-CSRF token
	checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

	// Get input
	$pass_new  = $_GET[ 'password_new' ];
	$pass_conf = $_GET[ 'password_conf' ];

	// Do the passwords match?
	if( $pass_new == $pass_conf ) {
		// They do!
		$pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
		$pass_new = md5( $pass_new );

		// Update the database
		$insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";
		$result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

		// Feedback for the user
		$html .= "<pre>Password Changed.</pre>";
	}
	else {
		// Issue with passwords matching
		$html .= "<pre>Passwords did not match.</pre>";
	}

	((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

// Generate Anti-CSRF token
generateSessionToken();

?>
```

1、正常修改密码并成功，使用BurpSuitePro捕获流量包，发现仍使用get提交方式，但多了验证参数token

 ![image-20240418174557916](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959106.png)

2、直接将链接复制粘贴修改密码肯定失败，提示token不正确

 ![image-20240418180900000](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959107.png)

有两种方法可以获得token值

方法一：利用DVWA中储存型XSS漏洞获得网页token

1、修改储存型XSS的网页脚本"D:\phpstudy_pro\DVWA-master\vulnerabilities\xss_s\index.php"，加入下面两行代码（修改文件之前做好备份）

```php+HTML
<div>Name:test<br/>Message:this is a test comment.<br/></div>
<div>Name:<iframe src='../csrf' οnlοad=alert(frames[0].document.getElementsByName('user_token')[0].value)><br/>Message:1<br/></div>
```

 ![屏幕截图 2024-04-18 190045](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959108.png)

2、保存文件，打开DVWA High级别下的储存型XSS页面，得到token值

 ![image-20240418190907301](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959109.png)

3、将得到的token替换，BurpSuitePro捕获流量包中原有的token

 ![image-20240418190836711](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959110.png)

4、发送构造好的数据包，得到响应，密码修改成功

 ![image-20240418191317461](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959111.png)

方法二：在BurpSuite安装插件，获取token

1、安装CSRF Token Tracker插件

 ![image-20240418184201655](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959112.png)

2、添加一条CSRF Token Tracker规则并勾选，再勾选"根据规则同步requests"

 ![image-20240615214659621](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959113.png)

3、截获修改密码请求包

 ![屏幕截图 2024-06-15 215508](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959114.png)

4、将抓到的包Send to Repeater，在Repeater页面修改"password_new"和"password_conf"参数，Send后发现token值发生变化，修改密码成功

 ![image-20240615215423256](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225164959115.png)

### DVWA Impossible 级别 CSRF

0、分析网页源代码

```php
<?php

if( isset( $_GET[ 'Change' ] ) ) {
	// Check Anti-CSRF token
	checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

	// Get input
	$pass_curr = $_GET[ 'password_current' ];
	$pass_new  = $_GET[ 'password_new' ];
	$pass_conf = $_GET[ 'password_conf' ];

	// Sanitise current password input
	$pass_curr = stripslashes( $pass_curr );
	$pass_curr = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_curr ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
	$pass_curr = md5( $pass_curr );

	// Check that the current password is correct
	$data = $db->prepare( 'SELECT password FROM users WHERE user = (:user) AND password = (:password) LIMIT 1;' );
	$data->bindParam( ':user', dvwaCurrentUser(), PDO::PARAM_STR );
	$data->bindParam( ':password', $pass_curr, PDO::PARAM_STR );
	$data->execute();

	// Do both new passwords match and does the current password match the user?
	if( ( $pass_new == $pass_conf ) && ( $data->rowCount() == 1 ) ) {
		// It does!
		$pass_new = stripslashes( $pass_new );
		$pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
		$pass_new = md5( $pass_new );

		// Update database with new password
		$data = $db->prepare( 'UPDATE users SET password = (:password) WHERE user = (:user);' );
		$data->bindParam( ':password', $pass_new, PDO::PARAM_STR );
		$data->bindParam( ':user', dvwaCurrentUser(), PDO::PARAM_STR );
		$data->execute();

		// Feedback for the user
		$html .= "<pre>Password Changed.</pre>";
	}
	else {
		// Issue with passwords matching
		$html .= "<pre>Passwords did not match or current password incorrect.</pre>";
	}
}

// Generate Anti-CSRF token
generateSessionToken();

?>

```