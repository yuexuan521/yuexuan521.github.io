---
title: "网络安全 DVWA通关指南 DVWA Weak Session IDs（弱会话）"
date: 2024-09-21 12:25:16
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "Web安全"
- "网络安全"
- "DVWA"
- "靶场"
---



## DVWA WeakSessionIDs（弱会话）

![DVWA 标志（文章封面）](/images/blog/20251225165536613.jpeg)





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

 ![Burp 抓到的请求，红框 dvwaSession=1](/images/blog/20251225165536615.png)

每重放一次，dvwaSession值加1。

 ![下一条请求 dvwaSession=2，说明取值可以被预测](/images/blog/20251225165536616.png)

 ![请求头里 Cookie 的 dvwaSession=4（高亮）](/images/blog/20251225165536617.png)

构造Payload：

```
dvwaSession=4; PHPSESSID=i2p425277d67521jah1hpkh3hr; security=low
```

使用火狐浏览器的hackbarV2，粘贴URL和cookie，提交(Execute)，实现免密码登录。

 ![HackBar 里设置 URL 与 Cookie dvwaSession=4 重放](/images/blog/20251225165536618.png)

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

 ![medium 级别 dvwaSession=1715927707，实际是时间戳](/images/blog/20251225165536619.png)

 ![同级别再点一次 Generate，仍是时间戳形式](/images/blog/20251225165536620.png)

2、获取对应时间的时间戳，拼接到cookie中提交，即可登录成功

 ![在线时间戳转换：1715927894 对应 2024-05-17 14:38:13](/images/blog/20251225165536621.png)

 ![HackBar 里把 Cookie 改成算出的时间戳](/images/blog/20251225165536622.png)

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

 ![high 级别 dvwaSession=c9f0f895fb98ab9159f51fd0297e236d，是 32 位 MD5](/images/blog/20251225165536623.png)

 ![MD5 反查该值，得到明文 8](/images/blog/20251225165536624.png)

2、将从0增加的整数进行MD5加密，MD5值作为cookie值，构造Payload提交：

```
dvwaSession=cfcd208495d565ef66e7dff9f98764da; dvwaSession=1715928053; PHPSESSID=26ks0v1tpvqsu15da00mn3i2cq; security=high
```

 ![MD5 工具把 0 加密成 cfcd208495d565ef66e7dff9f98764da](/images/blog/20251225165536625.png)

我的是新的页面，所以cookie值为0

 ![HackBar 同时带两个 dvwaSession，尝试覆盖旧值](/images/blog/20251225165536626.png)

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