---
title: "网络安全 DVWA通关指南 DVWA Command Injection（命令注入）"
date: 2024-09-26 12:23:57
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "安全"
- "网络安全"
- "网络攻击模型"
- "DVWA"
- "靶场"
---

## DVWACommand Injection（命令注入）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165229035.jpeg)





---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)

---

### Low

1、分析网页源代码

```php
<?php

// 当表单提交按钮（Submit）被触发时执行以下代码
if (isset($_POST['Submit'])) {

    // 获取用户通过POST方式提交的IP地址数据
    // 注意：此处使用$_REQUEST可能会受到GET和POST两种方式的影响，为了安全性建议明确指定来源（如$_POST）
    $target = $_REQUEST['ip'];

    // 检查当前服务器的操作系统类型
    if (stristr(php_uname('s'), 'Windows NT')) {
        // 如果是Windows操作系统，则构建用于执行ping命令的字符串
        // 使用单引号包围命令并在末尾添加从用户输入获取的IP地址
        // 注意：这段代码存在命令注入风险，因为未对$user变量进行任何过滤或转义
        $cmd = shell_exec('ping ' . $target);
    } else {
        // 否则，认为是类*nix系统（Unix/Linux/Mac OS等）
        // 构建用于执行ping命令的字符串，'-c 4' 参数表示发送4个ICMP请求包
        // 同样，这段代码也存在命令注入风险
        $cmd = shell_exec('ping -c 4 ' . $target);
    }

    // 将执行命令的结果赋值给 $cmd 变量，并将其作为HTML预格式化的文本显示给用户
    // 这里展示了命令执行结果，但也暴露了潜在的安全风险
    $html .= "<pre>{$cmd}</pre>";
}

?>
```

2、网页对参数没有任何过滤，可以使用"&“、”&&“、”|“、”||"逻辑连接符连接命令，直接执行命令。

> 连接符左右是否有空格没有影响
> 
> 注意逻辑连接符的区别

| 逻辑运算符 | 逻辑功能 |
|:---:|:---:|
| &(并且) | 有false则false |
| |(或者) | 有true则true |
| !(非) | 非false则true，非true则false |
| ^(异或) | 相同为false，不同为true |
| &&(短路与) | 有false则false,若&&左边表达式或者值为false则右边不进行计算 |
| ||(短路或) | 有true则true,若 |


```
192.168.90.127 && ipconfig
192.168.90.127 & ipconfig
0.0.0.0 || ipconfig
192.168.90.127 | ipconfig
```

 ![image-20240429163848470](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165229036.png)

4、为了消除命令注入风险，需要对用户输入进行严格的过滤或转义。例如，可以使用escapeshellarg()函数对目标IP地址进行转义，如下所示：

```php
// 对于Windows和*nix系统，都应先对用户输入进行转义
$target_sanitized = escapeshellarg($target);

// 然后构建命令
if (stristr(php_uname('s'), 'Windows NT')) {
    $cmd = shell_exec('ping ' . $target_sanitized);
} else {
    $cmd = shell_exec('ping -c 4 ' . $target_sanitized);
}
```

### Medium

1、分析网页源代码

```php
<?php

// 当检测到表单已提交（即点击了Submit按钮）时执行以下代码
if (isset($_POST['Submit'])) {

    // 获取用户通过POST方法提交的IP地址数据
    // 注意：这里使用$_REQUEST会同时接收GET和POST数据，若只期望POST数据，应使用$_POST['ip']
    $target = $_REQUEST['ip'];

    // 创建黑名单字符数组，其中包含了可能导致命令注入的特殊字符（在这里是逻辑运算符）
    $substitutions = array(
        '&&' => '', // 去除逻辑与符号，防止连续命令执行
        ';'  => '', // 去除分号，防止多条命令执行
    );

    // 使用str_replace函数替换掉用户输入中黑名单内的字符
    // 这是一个初级防护措施，但并不能完全阻止所有类型的命令注入攻击
    $target = str_replace(array_keys($substitutions), $substitutions, $target);

    // 检测当前服务器的操作系统类型
    if (stristr(php_uname('s'), 'Windows NT')) {
        // 如果是Windows操作系统，则执行ping命令
        $cmd = shell_exec('ping ' . $target);
    } else {
        // 否则，认为是类*nix系统（Unix/Linux/Mac OS等）
        // 执行带有-c参数的ping命令，表示向目标主机发送4个数据包
        $cmd = shell_exec('ping -c 4 ' . $target);
    }

    // 将ping命令的输出结果以HTML预格式化的文本形式呈现给用户
    // 虽然进行了部分字符过滤，但仍然需要注意此代码仍可能存在命令注入风险
    $html .= "<pre>{$cmd}</pre>";
}

?>
```

2、网页将"&&"连接符过滤了，可以使用其他的逻辑连接符，命令注入成功。

```
192.168.90.127 & ipconfig
0.0.0.0 || ipconfig
192.168.90.127 | ipconfig
```

 ![image-20240429171008470](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165229037.png)

### High

1、分析网页源代码

```php
<?php

// 当检测到表单已提交（Submit按钮已被点击）时执行以下代码
if (isset($_POST['Submit'])) {

    // 获取用户提交的IP地址输入，并使用trim函数去除首尾空白字符
    $target = trim($_REQUEST['ip']);

    // 定义一个黑名单字符数组，包含一些可能用于命令注入的特殊字符
    $substitutions = array(
        '&'   => '',     // 空字符替换"&"（逻辑与符号，用于连接多个命令）
        ';'   => '',     // 空字符替换";"（命令分隔符，用于执行多条命令）
        '| '  => '',     // 空字符替换"| "（管道符号，用于命令间通信）！！！我真的没看到这里居然有一个空格！！！
        '-'   => '',     // 空字符替换"-"（某些命令中的选项标志或组合命令）
        '$'   => '',     // 空字符替换"$"（环境变量引用或bash命令执行）
        '('   => '',     // 空字符替换"("（子shell执行或命令组）
        ')'   => '',     // 空字符替换")"（与"("配套使用）
        '`'   => '',     // 空字符替换"`"（命令替换）
        '||'  => '',     // 空字符替换"||"（逻辑或符号，用于命令执行失败时执行下一条命令）
    );

    // 使用str_replace函数，将用户输入中黑名单内所有字符替换为空字符
    // 这是一种针对命令注入的基本防御措施，但无法保证完全抵御所有攻击手法
    $target = str_replace(array_keys($substitutions), $substitutions, $target);

    // 判断当前操作系统类型
    if (stristr(php_uname('s'), 'Windows NT')) {
        // 若是Windows操作系统，则执行ping命令
        $cmd = shell_exec('ping ' . $target);
    } else {
        // 否则，认为是类*nix系统（Unix/Linux/Mac OS等）
        // 执行带有-c参数的ping命令，表示向目标主机发送4个数据包
        $cmd = shell_exec('ping -c 4 ' . $target);
    }

    // 将ping命令执行的原始输出反馈给用户，以HTML预格式化的文本形式展示
    // 尽管进行了字符过滤，但此代码依然存在命令注入的风险
    $html .= "<pre>{$cmd}</pre>";
}

?>
```

2、真没想到黑名单字符数组中，'| ''的后面多了一个空格，所以还是可以使用"|"连接符进行连接。

```
192.168.90.127 |ipconfig
```

 ![image-20240429174030718](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165229038.png)

### Impossible

```php
<?php

// 当表单提交按钮（Submit）被触发时执行以下代码
if (isset($_POST['Submit'])) {

    // 验证Anti-CSRF令牌，防止跨站请求伪造攻击
    checkToken($_REQUEST['user_token'], $_SESSION['session_token'], 'index.php');

    // 获取用户输入的IP地址，并使用stripslashes函数去除反斜杠（\）以防止魔术引号攻击
    $target = $_REQUEST['ip'];
    $target = stripslashes($target);

    // 将IP地址拆分为四个八位字节（点分十进制形式）
    $octet = explode(".", $target);

    // 检查每个八位字节是否都是整数
    if (
        (is_numeric($octet[0])) &&
        (is_numeric($octet[1])) &&
        (is_numeric($octet[2])) &&
        (is_numeric($octet[3])) &&
        (sizeof($octet) == 4)
    ) {
        // 如果所有四个八位字节均为整数，则重新组合IP地址
        $target = $octet[0] . '.' . $octet[1] . '.' . $octet[2] . '.' . $octet[3];

        // 根据操作系统类型执行ping命令
        if (stristr(php_uname('s'), 'Windows NT')) {
            // 如果是Windows操作系统
            $cmd = shell_exec('ping ' . $target);
        } else {
            // 如果是*nix系统（如Unix/Linux/Mac OS）
            $cmd = shell_exec('ping -c 4 ' . $target);
        }

        // 将ping命令执行结果以HTML预格式化文本的形式返回给用户
        $html .= "<pre>{$cmd}</pre>";
    } else {
        // 用户输入的不是有效的IP地址，显示错误消息
        $html .= '<pre>ERROR: You have entered an invalid IP.</pre>';
    }
}

// 生成新的Anti-CSRF令牌并存储到session中
generateSessionToken();

?>

**注释说明：**

- 此PHP脚本主要处理用户提交的IP地址，并执行ping命令检查其连通性。
- 使用`checkToken`函数验证用户提交的Anti-CSRF令牌，确保请求来自合法用户而非第三方恶意伪造。
- 获取用户输入的IP地址，并通过`stripslashes`函数移除可能存在的反斜杠，以防止SQL注入或其他基于字符串逃逸的攻击。
- 将IP地址拆分成四个八位字节，然后逐一检查它们是否为数字，确保IP地址格式正确。
- 根据服务器操作系统类型执行相应的ping命令，并将结果显示给用户。
- 在脚本末尾调用`generateSessionToken`函数生成新的Anti-CSRF令牌，为后续请求提供保护。
```