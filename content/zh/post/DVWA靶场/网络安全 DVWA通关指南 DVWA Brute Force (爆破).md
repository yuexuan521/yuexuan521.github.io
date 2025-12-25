---
title: "网络安全 DVWA通关指南 DVWA Brute Force (爆破)"
date: 2024-09-26 12:24:23
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "android"
- "安全"
- "网络安全"
- "爆破"
- "密码学"
- "DVWA"
---



## DVWA Brute Force (爆破)

![在这里插入图片描述](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113338.jpeg)





---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)



---

### Low

1、分析网页源代码

```php
<?php

// 检查是否存在"Login" GET 参数，这通常是提交登录表单后触发的动作
if( isset( $_GET[ 'Login' ] ) ) {

    // 获取POST方式提交的用户名
    $user = $_GET[ 'username' ]; // 注意：这里应当使用 $_POST 而非 $_GET 来获取表单数据，因为登录通常涉及敏感信息，推荐使用 POST 方法

    // 获取POST方式提交的密码，并使用md5函数对其进行哈希加密（注意：MD5已经不再安全，应使用更安全的加密算法如bcrypt）
    $pass = $_GET[ 'password' ]; // 同上，此处应改为 $_POST['password']
    $pass = md5( $pass ); // 这里假设密码在数据库中是以MD5形式存储的

    // 创建SQL查询语句，检查数据库中是否存在匹配的用户名和密码
    $query  = "SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';";

    // 执行SQL查询，连接数据库并处理潜在错误
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( 
        '<pre>' . 
        ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : 
        (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . 
        '</pre>'
    );

    // 检查查询结果是否成功且只有一条记录匹配（意味着用户名和密码正确）
    if( $result && mysqli_num_rows( $result ) == 1 ) {

        // 获取匹配用户的详细信息
        $row = mysqli_fetch_assoc( $result );
        
        // 提取用户头像URL
        $avatar = $row["avatar"];

        // 登录成功，构造欢迎消息并显示用户头像
        $html .= "<p>Welcome to the password protected area {$user}</p>";
        $html .= "<img src=\"{$avatar}\" />"; // 显示用户的头像图片

    } else {

        // 登录失败，输出错误提示信息
        $html .= "<pre><br />Username and/or password incorrect.</pre>";

    }

    // 关闭数据库连接
    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);

} // 结束 if(isset($_GET['Login']))

?>
```

2、使用管理员admin登录，密码尝试123，提示错误

 ![image-20240516161258388](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113339.png)

使用Burp Suite抓包，将数据包发给Intruder（测试器），选择Sniper（狙击手）模式，选择password为有效载荷。

> **单字典(只有一个字典)** 
> 1.Sniper：按顺序一个一个参数依次遍历。
> 2.Battering ram：每个参数同时遍历同一个字典，两个参数的值相同。
> 
> **多字典(有多少参数就有多少字典）** 
> 1.Pitchfork：多个参数同时进行遍历，只是一个选字典1，一个选字典2（相当于50m赛跑同时出发，只是赛道不同，互不干扰。爆破次数取决于最短的字典长度）
> 2.Cluster bomb：有点像两个嵌套的for循环，参数i和参数j，i=0，然后j要从0-10全部跑完，然后i=1，然后j再从0-10跑完，一对多，多次遍历

 ![image-20240516161616103](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113340.png)

 ![image-20240430155738815](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113341.png)

使用字典进行爆破，字典可以自己制作，也可以网上直接下载，等待片刻爆破完成，使用爆破出的密码就能登录。

 ![image-20240516173612338](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113342.png)

 ![image-20240516173933317](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113343.png)

### Medium

1、分析网页源代码

```php
<?php

if( isset( $_GET[ 'Login' ] ) ) {
	// Sanitise username input
	$user = $_GET[ 'username' ];
	$user = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $user ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

	// Sanitise password input
	$pass = $_GET[ 'password' ];
	$pass = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
	$pass = md5( $pass );

	// Check the database
	$query  = "SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

	if( $result && mysqli_num_rows( $result ) == 1 ) {
		// Get users details
		$row    = mysqli_fetch_assoc( $result );
		$avatar = $row["avatar"];

		// Login successful
		$html .= "<p>Welcome to the password protected area {$user}</p>";
		$html .= "<img src=\"{$avatar}\" />";
	}
	else {
		// Login failed
		sleep( 2 ); // 当登录验证失败时界面将睡眠 2 秒
		$html .= "<pre><br />Username and/or password incorrect.</pre>";
	}

	((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>

```

2、密码验证方面，增加验证失败睡眠两秒的限制，这会加大爆破所需要的时间。但只要时间充足，爆破出密码不是问题。

试了一下，果然很慢。

 ![image-20240516174443533](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113344.png)

### High

1、分析网页源代码

```php
<?php

if( isset( $_GET[ 'Login' ] ) ) {
	// Check Anti-CSRF token
	checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

	// Sanitise username input
	$user = $_GET[ 'username' ];
	$user = stripslashes( $user );
	$user = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $user ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

	// Sanitise password input
	$pass = $_GET[ 'password' ];
	$pass = stripslashes( $pass );
	$pass = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
	$pass = md5( $pass );

	// Check database
	$query  = "SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

	if( $result && mysqli_num_rows( $result ) == 1 ) {
		// Get users details
		$row    = mysqli_fetch_assoc( $result );
		$avatar = $row["avatar"];

		// Login successful
		$html .= "<p>Welcome to the password protected area {$user}</p>";
		$html .= "<img src=\"{$avatar}\" />";
	}
	else {
		// Login failed
		sleep( rand( 0, 3 ) );
		$html .= "<pre><br />Username and/or password incorrect.</pre>";
	}

	((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

// Generate Anti-CSRF token
generateSessionToken();

?>

```

2、进入Position模块，选择Attacktype为Pitchfork模式，选择password和user_token为爆破对象

 ![image-20240605111002633](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113345.png)

进入Resource Pool模块，

 ![image-20240605111117373](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113346.png)

进入Options模块，找到Grep - Extract选项卡，添加一个正则表达式匹配返回的user_token

 ![image-20240605111153105](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113347.png)

点击Refetch response，从response中找到user_token并选中

 ![image-20240605111312704](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113348.png)

 ![image-20240605111501473](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113349.png)

载入字典

 ![image-20240605111405891](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113350.png)

第二个参数"token"选择从返回包匹配，填入当前token

 ![image-20240605111547847](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113351.png)

爆破成功，登录成功

 ![image-20240605110902881](https://raw.githubusercontent.com/yuexuan521/image/main/20251225165113352.png)

### Impossible

```php
<?php

if( isset( $_POST[ 'Login' ] ) ) {
	// Check Anti-CSRF token
	checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

	// Sanitise username input
	$user = $_POST[ 'username' ];
	$user = stripslashes( $user );
	$user = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $user ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

	// Sanitise password input
	$pass = $_POST[ 'password' ];
	$pass = stripslashes( $pass );
	$pass = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
	$pass = md5( $pass );

	// Default values
	$total_failed_login = 3;
	$lockout_time       = 15;
	$account_locked     = false;

	// Check the database (Check user information)
	$data = $db->prepare( 'SELECT failed_login, last_login FROM users WHERE user = (:user) LIMIT 1;' );
	$data->bindParam( ':user', $user, PDO::PARAM_STR );
	$data->execute();
	$row = $data->fetch();

	// Check to see if the user has been locked out.
	if( ( $data->rowCount() == 1 ) && ( $row[ 'failed_login' ] >= $total_failed_login ) )  {
		// User locked out.  Note, using this method would allow for user enumeration!
		//$html .= "<pre><br />This account has been locked due to too many incorrect logins.</pre>";

		// Calculate when the user would be allowed to login again
		$last_login = $row[ 'last_login' ];
		$last_login = strtotime( $last_login );
		$timeout    = strtotime( "{$last_login} +{$lockout_time} minutes" );
		$timenow    = strtotime( "now" );

		// Check to see if enough time has passed, if it hasn't locked the account
		if( $timenow > $timeout )
			$account_locked = true;
	}

	// Check the database (if username matches the password)
	$data = $db->prepare( 'SELECT * FROM users WHERE user = (:user) AND password = (:password) LIMIT 1;' );
	$data->bindParam( ':user', $user, PDO::PARAM_STR);
	$data->bindParam( ':password', $pass, PDO::PARAM_STR );
	$data->execute();
	$row = $data->fetch();

	// If its a valid login...
	if( ( $data->rowCount() == 1 ) && ( $account_locked == false ) ) {
		// Get users details
		$avatar       = $row[ 'avatar' ];
		$failed_login = $row[ 'failed_login' ];
		$last_login   = $row[ 'last_login' ];

		// Login successful
		$html .= "<p>Welcome to the password protected area <em>{$user}</em></p>";
		$html .= "<img src=\"{$avatar}\" />";

		// Had the account been locked out since last login?
		if( $failed_login >= $total_failed_login ) {
			$html .= "<p><em>Warning</em>: Someone might of been brute forcing your account.</p>";
			$html .= "<p>Number of login attempts: <em>{$failed_login}</em>.<br />Last login attempt was at: <em>${last_login}</em>.</p>";
		}

		// Reset bad login count
		$data = $db->prepare( 'UPDATE users SET failed_login = "0" WHERE user = (:user) LIMIT 1;' );
		$data->bindParam( ':user', $user, PDO::PARAM_STR );
		$data->execute();
	}
	else {
		// Login failed
		sleep( rand( 2, 4 ) );

		// Give the user some feedback
		$html .= "<pre><br />Username and/or password incorrect.<br /><br/>Alternative, the account has been locked because of too many failed logins.<br />If this is the case, <em>please try again in {$lockout_time} minutes</em>.</pre>";

		// Update bad login count
		$data = $db->prepare( 'UPDATE users SET failed_login = (failed_login + 1) WHERE user = (:user) LIMIT 1;' );
		$data->bindParam( ':user', $user, PDO::PARAM_STR );
		$data->execute();
	}

	// Set the last login time
	$data = $db->prepare( 'UPDATE users SET last_login = now() WHERE user = (:user) LIMIT 1;' );
	$data->bindParam( ':user', $user, PDO::PARAM_STR );
	$data->execute();
}

// Generate Anti-CSRF token
generateSessionToken();

?>

```