---
title: "网络安全 DVWA通关指南 DVWA Reflected Cross Site Scripting (反射型 XSS)"
date: 2024-09-26 12:22:37
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "xss"
- "安全"
- "网络安全"
- "DVWA"
- "反射型 XSS"
- "网络攻击模型"
---



## DVWAReflected Cross Site Scripting (反射型 XSS)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165408561.jpeg)



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
> #### 反射型
> 
> 出现在搜索栏，用户登录等地方，常用来窃取客户端的Cookie进行钓鱼欺骗。(需要用户去点击)
> 
> 想要窃取cookie要满足两个条件：
> 
> > 1.用户点击攻击者构造的URL
> > 
> > 2.访问被攻击的应用服务(即存在xss的网站)
> 
> 

### Low

1、分析网页源代码

```php
<?php

header ("X-XSS-Protection: 0");
//这行代码实际上禁用了浏览器内置的XSS防护机制。现代浏览器通常会有一个XSS过滤器，默认开启，用于检测并阻止某些类型的反射型XSS攻击。将此值设为0意味着告诉浏览器不要进行任何自动的XSS防护。

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
	// Feedback for end user
	$html .= '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';
}

?>
//对用户输入的数据没有进行任何过滤或转义处理
```

2、输入 `<script>alert(/XSS/)</script>` ，弹出一个警告框显示“XSS”，这证明了XSS攻击的成功。

```
1、<script>alert(/XSS/)</script>

<script> 和 </script> 是HTML中的标签，用于定义JavaScript代码块的开始和结束。
alert() 是JavaScript的一个内置函数，用于显示带有一条消息的对话框。用户必须点击确定按钮才能关闭这个对话框并继续操作页面。
```

 ![image-20240424173639685](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165408562.png)

 ![image-20240424173657851](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165408563.png)

3、 **解决方案** :

- 对用户输入的数据进行适当的过滤或转义，可以使用PHP的 `htmlspecialchars()` 函数来转义特殊字符，确保它们被安全地显示为数据而不是被执行为代码：

  ```php
  1$safeName = htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8');
  2$html .= '<pre>Hello ' . $safeName . '</pre>';
  ```

- 同时，考虑是否真的需要禁用XSS防护头，除非有充分的理由，否则应保持浏览器的默认防护机制启用。

### Medium

1、分析网页源代码

```php
<?php

header ("X-XSS-Protection: 0");

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
	// Get input
	$name = str_replace( '<script>', '', $_GET[ 'name' ] );

	// Feedback for end user
	$html .= "<pre>Hello ${name}</pre>";
}

?>
//str_replace() 函数以其他字符替换字符串中的一些字符（区分大小写）。本例中的作用为将'<script>'替换为''。
```

2、因为str_replace() 函数区分大小写，可以将 `<script>` 中的字符改成大写

```
<scr<script>ipt>alert(/XSS/)</script>
//双写绕过
<ScrIpt>alert(/XSS/)</scRiPt>
//区分大小写
<script x>alert(/XSS/)</script y>
//绕过<script>
```

 ![image-20240424181634960](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165408564.png)

 ![image-20240424181648248](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165408565.png)

### High

1、分析网页源代码

```php
<?php

header ("X-XSS-Protection: 0");

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
	// Get input
	$name = preg_replace( '/<(.*)s(.*)c(.*)r(.*)i(.*)p(.*)t/i', '', $_GET[ 'name' ] );
// preg_replace 是 PHP 中的一个函数，用于执行正则表达式的搜索和替换操作。
// 正则表达式：/<(.*)s(.*)c(.*)r(.*)i(.*)p(.*)t/i
// /<...>/：定义了一个正则表达式的匹配模式，前后尖括号表示匹配任意字符直到遇到后面的字符。
// (.*)：点号.表示匹配任意单个字符（除了换行符），后面跟着的*表示前面的模式可以重复零次或多次。因此，(.*?)组合在一起表示匹配任意数量的任意字符，但这里的懒惰量词*?并没有使用，实际是贪婪匹配.*。
// s, c, r, i, p, t：分别匹配这些字母，中间的.和*允许任意字符出现在这些字母之间。
// /i：这是一个修饰符，表示执行不区分大小写的匹配。
    
	// Feedback for end user
	$html .= "<pre>Hello ${name}</pre>";
}

?>
// <script>标签被完全过滤
```

2、使用其它标签注入 JS 攻击脚本

```
1、<img src=x onerror=alert(/XSS/)>

<img src=x onerror=alert(1)> 这段代码是一种典型的跨站脚本攻击（XSS）示例，利用了HTML图像标签（<img>）的onerror事件来触发JavaScript代码执行。其工作原理如下：

图像标签（<img>）：此标签用于在网页中嵌入图片。正常情况下，src属性会指向一个图像文件的URL。

src=x：这里的src属性被赋值为x，这是一个无效的图像URL。当浏览器尝试根据这个无效的URL加载图片时，自然找不到对应的资源，从而触发错误。

onerror事件：这是HTML元素的一个事件属性，当指定的错误情况发生时（如图像加载失败），会执行紧跟在其后的JavaScript代码。在这个例子中，就是alert(1)。

alert(1)：这是一个简单的JavaScript语句，用于弹出一个包含数字1的警告框。在实际的XSS攻击中，这可能被替换为更复杂的恶意代码，用于盗取用户Cookie、重定向用户到恶意站点、执行恶意脚本等。


2、<svg onload=alert(/XSS/)>

<svg onload=alert(/xss/)> 这段代码展示了另一种跨站脚本攻击（XSS）的载体，这次是利用SVG（可缩放矢量图形）元素的onload事件来触发JavaScript代码执行。其工作原理如下：

SVG元素：SVG是一种用于定义矢量图形的XML标记语言，可以直接嵌入到HTML文档中。与普通的HTML元素一样，SVG元素也支持事件处理器，如onload。

onload事件：在SVG中，onload事件会在SVG文档或者图像加载完成后触发。与HTML的<body onload="">类似，它提供了一个时机来执行指定的JavaScript代码。

alert(/xss/)：这里使用的JavaScript代码与之前的例子相似，目的是弹出一个警告框显示/xss/。但这里的/xss/实际上是正则表达式字面量，虽然作为alert函数的参数，它会被当作普通字符串显示，这不影响弹出警告框的效果，但表明攻击者可以嵌入更复杂的JavaScript逻辑，不仅仅是简单的字符串。

攻击原理：当这段SVG代码被嵌入到一个网页中，一旦SVG图形加载完成，onload事件就会触发，并执行alert(/xss/), 弹出一个包含/xss/的警告框。实质上，这揭示了网页对用户输入数据处理不当，允许执行恶意脚本的风险。攻击者可以利用这一点，不仅限于弹窗，还可以执行任何JavaScript代码，实现更深层次的攻击，如窃取用户数据、操控页面内容、发起进一步的攻击等。
```

 ![image-20240425123935752](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165408566.png)

### Impossible

```php
<?php

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

    // Get input
    $name = htmlspecialchars( $_GET[ 'name' ] );

    // Feedback for end user
    $html .= "<pre>Hello ${name}</pre>";
}

// Generate Anti-CSRF token
generateSessionToken();

?>
```