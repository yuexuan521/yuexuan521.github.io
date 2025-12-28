---
title: "BUUCTF [GXYCTF2019]佛系青年 1"
date: 2025-08-12 00:15:46
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "CTF"
- "Misc"
- "BUUCTF"
- "与佛论禅"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191216628.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191219294.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到ZIP压缩包。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191221432.png)

---

### 解题思路：

1、压缩包内有一张png图片和一个txt文本，解压zip压缩包，解压出图片，但txt文本提示需要输入密码。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191222835.png)

解压出的png图片

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191358437.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191400614.png)

2、压缩包内有两个文件，而且已经解压出了一个文件，我猜测为zip压缩包明文攻击，但后面没有成功解出密码。看过别人的题解之后，发现原来是zip伪加密。
通过010Editor修改压缩源文件数据区和目录区的全局方式位标记（下图红色标识），将伪压缩文件恢复到未加密的状态。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191402433.png)

保存文件，解压得到fo.txt文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191404239.png)

```bash
                                                                      _ooOoo_
                                                                     o8888888o
                                                                     88" . "88
                                                                     (| -_- |)
                                                                      O\ = /O
                                                                  ____/`---'\____
                                                                .   ' \\| |// `.
                                                                 / \\||| : |||// \
                                                               / _||||| -:- |||||- \
                                                                 | | \\\ - /// | |
                                                               | \_| ''\---/'' | |
                                                                \ .-\__ `-` ___/-. /
                                                             ___`. .' /--.--\ `. . __
                                                          ."" '< `.___\_<|>_/___.' >'"".
                                                         | | : `- \`.;`\ _ /`;.`/ - ` : | |
                                                           \ \ `-. \_ __\ /__ _/ .-` / /
                                                   ======`-.____`-.___\_____/___.-`____.-'======
                                                                      `=---='            

                                                   .............................................
                                                          佛祖保佑             永无BUG
                                                          写字楼里写字间，写字间里程序员；
                                                          程序人员写程序，又拿程序换酒钱。
                                                          酒醒只在网上坐，酒醉还来网下眠；
                                                          酒醉酒醒日复日，网上网下年复年。
                                                          但愿老死电脑间，不愿鞠躬老板前；
                                                          奔驰宝马贵者趣，公交自行程序员。
                                                          别人笑我忒疯癫，我笑自己命太贱；
                                                          不见满街漂亮妹，哪个归得程序员？

佛曰：遮等諳勝能礙皤藐哆娑梵迦侄羅哆迦梵者梵楞蘇涅侄室實真缽朋能。奢怛俱道怯都諳怖梵尼怯一罰心缽謹缽薩苦奢夢怯帝梵遠朋陀諳陀穆諳所呐知涅侄以薩怯想夷奢醯數羅怯諸
```

3、打开fo.txt文件，如上图。判断文件底部的那一长串文字，为经过“与佛论禅”加密的密文，通过在线网站解密，得到flag。（这个文本真的爱了！）
[与佛论禅密码](https://ctf.bugku.com/tool/todousharp) 

```bash
佛曰：遮等諳勝能礙皤藐哆娑梵迦侄羅哆迦梵者梵楞蘇涅侄室實真缽朋能。奢怛俱道怯都諳怖梵尼怯一罰心缽謹缽薩苦奢夢怯帝梵遠朋陀諳陀穆諳所呐知涅侄以薩怯想夷奢醯數羅怯諸
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191406093.png)

**更正** 
在线网站已经无法使用，可以下载这个工具进行解码。
[https://github.com/qianxiao996/CTF-Tools](https://github.com/qianxiao996/CTF-Tools) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191407660.png)

### flag：

```bash
flag{w0_fo_ci_Be1}
```