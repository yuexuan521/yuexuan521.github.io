---
title: "网络安全 DVWA通关指南 DVWA Weak Session IDs（弱会话）"
date: 2024-09-21 12:25:16
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "安全"
- "网络安全"
- "DVWA"
- "靶场"
- "弱会话"
- "系统安全"
---



## DVWA WeakSessionIDs（弱会话）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536613.jpeg)





---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)

---



### Low Level

1、分析网页源代码

```php
<?php

$html = "";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    if (!isset ($_SESSION['last_session_id'])) {
       $_SESSION['last_session_id'] = 0;
    }
    $_SESSION['last_session_id']++;
    $cookie_value = $_SESSION['last_session_id'];
    setcookie("dvwaSession", $cookie_value);
}
?>
```

Low级别的cookie生成方式：如果 $cookie_value不存在就设为0，存在则$ cookie_value加1，最后以dvwaSession=$cookie_value呈现。

2、使用BurpSuite抓包，如下：

 ![image-20240517140744204](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536615.png)

每重放一次，dvwaSession值加1。

 ![image-20240517141306684](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536616.png)

 ![image-20240517142357483](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536617.png)

构造Payload：

```
dvwaSession=4; PHPSESSID=i2p425277d67521jah1hpkh3hr; security=low
```

使用火狐浏览器的hackbarV2，粘贴URL和cookie，提交(Execute)，实现免密码登录。

 ![image-20240517142320371](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536618.png)

### Medium Level

1、分析网页源代码

```php
<?php

$html = "";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    $cookie_value = time();
    //time() 函数返回自 Unix 纪元（January 1 1970 00:00:00 GMT）起的当前时间的秒数。
    setcookie("dvwaSession", $cookie_value);
}
?>
```

Medium Level的cookie值由时间戳生成。抓包如下：

 ![image-20240517143656163](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536619.png)

 ![image-20240517143721534](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536620.png)

2、获取对应时间的时间戳，拼接到cookie中提交，即可登录成功

 ![image-20240517144322688](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536621.png)

 ![image-20240517144204048](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536622.png)

### High Level

1、分析网页源代码

```php
<?php

$html = "";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    if (!isset ($_SESSION['last_session_id_high'])) {
       $_SESSION['last_session_id_high'] = 0;
    }
    $_SESSION['last_session_id_high']++;
    $cookie_value = md5($_SESSION['last_session_id_high']);
    setcookie("dvwaSession", $cookie_value, time()+3600, "/vulnerabilities/weak_id/", $_SERVER['HTTP_HOST'], false, false);
}

?>
```

cookie值的初始生成与Low level一致，对cookie值进行MD5加密后作为cookie值。抓包如下：

 ![image-20240517145842879](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536623.png)

 ![image-20240517145810334](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536624.png)

2、将从0增加的整数进行MD5加密，MD5值作为cookie值，构造Payload提交：

```
dvwaSession=cfcd208495d565ef66e7dff9f98764da; dvwaSession=1715928053; PHPSESSID=26ks0v1tpvqsu15da00mn3i2cq; security=high
```

 ![image-20240517150947017](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536625.png)

我的是新的页面，所以cookie值为0

 ![image-20240517151113764](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165536626.png)

### Impossible Level

```php
<?php

$html = "";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    $cookie_value = sha1(mt_rand() . time() . "Impossible");
    setcookie("dvwaSession", $cookie_value, time()+3600, "/vulnerabilities/weak_id/", $_SERVER['HTTP_HOST'], true, true);
}
?>
```