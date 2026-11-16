---
title: "BUUCTF [GKCTF 2021]你知道apng吗 1"
date: 2025-01-19 11:53:54
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "网络安全"
- "BUUCTF"
- "MISC"
- "隐写"
- "Web安全"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190956824.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [Hello CTF](https://hello-ctf.com/HC_Start/)
- [NewStar CTF](https://ns.openctf.net/learn/)
- [buuctf ［GKCTF 2021］你知道apng吗 ＜apng图片格式的考察＞](https://blog.csdn.net/weixin_45556441/article/details/119801433)
- [buuctf-misc-［GKCTF 2021］你知道apng吗1](https://blog.csdn.net/qq_29977871/article/details/128507643)
- [你知道apng吗](https://blog.csdn.net/wangjin7356/article/details/123402038)


 ![题目页「［GKCTF 2021］你知道apng吗 1」，附件 girl.apng](/images/blog/20251228190958970.png)

### 题目描述：

（flag由flag头包裹

### 密文：

下载附件，解压得到girl.apng文件。

 ![girl.apng，1,149 KB，显示为 APNG 文件](/images/blog/20251228191000373.png)

---

### 解题思路：

1、根据题目 `你知道apng吗` ，搜索得到这是一种类似于GIF的动图文件。

[科普（APNG）](https://www.bilibili.com/opus/301546385385594815) 

> APNG：我出身于PNG（便携式网络图形），我的使命就是淘汰GIF。我的全称是Animated Portable Network Graphics，我的本质是PNG的位图动画扩展，但未获PNG组织官方认可。我仍对原版PNG保持向下兼容，我的第1帧为标准PNG图像，剩余的动画和帧速等数据放在PNG扩展数据块，因此只支持原版PNG的软件会正确显示第1帧。Firefox与Chrome浏览器（59版本及以上）均可以直接打开我。

[APNG百度](https://baike.baidu.com/item/APNG/3582613) 

> APNG（Animated Portable Network Graphics）是一个基于PNG（Portable Network Graphics）的位图动画格式，扩展方法类似主要用于网页的GIF 89a，仍对传统PNG保留向下兼容。第1帧是标准的单幅PNG图像，因此只支持原版PNG的软件能正常显示第1帧。剩余的动画帧和帧速数据储存在符合原版PNG标准的扩展数据块里。 APNG与Mozilla社区关系密切，格式标准文档就放在Mozilla网站。

使用Firefox可以打开girl.apng，可以看到有多个二维码一闪而过。

找到一个APNG在线查看工具： [https://ezgif.com/split?err=expired](https://ezgif.com/split?err=expired) ，上传图片后，点击 `Split` ，得到每帧的图片。

 ![在线 GIF frame extractor，工具栏红框 split，底部「Split to frames!」](/images/blog/20251228191001456.png)

---

`1、` 
 ![其中一帧：女孩抱着花，右边浮出半张倾斜的二维码](/images/blog/20251228191003482.png)

`2、` 
 ![另一帧：女孩起跳，背后隐约透出二维码](/images/blog/20251228191005565.png)

`3、` 
 ![有一帧显示成「该图片可能违规或链接失效」](/images/blog/20251228191007640.png)

`4、` 
 ![紧接着还有一张同样的占位图](/images/blog/20251228191009559.png)

---

2、使用PS（或在线PS）处理第一张图片，选择 `编辑` –> `变换` –> `扭曲` ，此时扫码得到四分之一的flag。

 ![Photoshop 编辑 → 变换 → 扭曲，把斜着的二维码拉正](/images/blog/20251228191010855.png)

 ![QR Research 解出第一段 flag{a3c7e4e5](/images/blog/20251228191012920.png)

第二张图片使用StegSolve，查看其它的通道，找到可以识别的二维码。

 ![StegSolve 切 Green plane 0，噪点里浮出一个小二维码](/images/blog/20251228191014992.png)

 ![解出 -9b9d](/images/blog/20251228191017077.png)

剩下两张可以直接扫码得到四分之一flag，最后得到完整flag。

 ![另一帧原图里的二维码解出 -ad20，颜色反色且镜像](/images/blog/20251228191018883.png)

 ![最后一帧解出 -0327-288a235370ea}，四段拼起来才是完整 flag](/images/blog/20251228191021077.png)

### flag：

```bash
flag{a3c7e4e5-9b9d-ad20-0327-288a235370ea}
```