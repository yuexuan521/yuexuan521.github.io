---
title: "BUUCTF [GUET-CTF2019]soul sipse 1"
date: 2025-03-13 11:23:36
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "flag"
- "安全"
- "网络安全"
- "buuctf"
- "Steghide"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191044084.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[CTF 隐写工具Steghide](https://blog.csdn.net/Aluxian_/article/details/121968033) 
[BUUCTF：[GUET-CTF2019]soul sipse](https://blog.csdn.net/mochu7777777/article/details/109838676) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191046248.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到out.wav

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191047951.png)

---

### 解题思路：

1、out.wav文件使用了Steghide工具进行隐写。

> Steghide支持以下图像格式：JPEG，BMP，WAV，AU文件。

安装Steghide命令（Kali）：

```shell
apt-get install  steghide
```

使用Steghide查看out.wav文件，发现隐写了一个download.txt文件，提取出来是一条链接。

```bash
https://share.weiyun.com/5wVTIN3
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191049010.png)

```powershell
steghide info out.wav
steghide extract -sf out.wav
cat download.txt 
```

访问该链接，到腾讯微云下载一张图片GUET.png。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191051039.png)

2、图片无法打开，用010Editor打开，发现png文件头错误，修改为 `89 50 4E 47` ，保存可正常浏览。

> PNG (png) 文件头：89 50 4E 47

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191052361.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191053792.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191055329.png)

正常图片：
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191057015.png)

3、图片中的内容为，经过Unicode编码后的数据。

```bash
\u0034\u0030\u0037\u0030\u000d\u000a\u0031\u0032\u0033\u0034\u000d\u000a
```

Unicode解码：
[Unicode编解码](https://tool.chinaz.com/tools/unicode.aspx) 

```bash
4070
1234
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191058516.png)

最后，将得到数据相加，得到flag： `5304` .

```python
  4070
+ 1234
-------
  5304
```

### flag：

```bash
flag{5304}
```

## 🔒 玥轩 | 网络安全拓扑空间

#### 🛡️ 攻防演化方程

`#CTF_TOP10% | #misc | #密码学 | #漏洞宇宙` 

---

### 🔑 安全算法素因子分解

**漏洞扫描定理** ：

$$
\sqrt{61526} \approx 248 \quad \Rightarrow \quad \text{61,526次代码片分享构建248个攻防模型}
$$

**竞赛场博弈论** ：

$$
\begin{cases} \text{CTF攻防矩阵} & \nabla \cdot \vec{F} = \rho/\epsilon_0 \\ \text{数模国赛优化解} & \iint_{D} e^{x^2+y^2} \, dx \, dy \end{cases}
$$

---

### 🛰️ 云安全算子矩阵

$$
\text{专家算子} = \begin{bmatrix} \text{阿里云} & \otimes & \text{乘风者计划} \\ \text{华为云} & \oplus & \text{云享专家} \\ \end{bmatrix}^{\mathrm{T}}
$$

---

### 🧩 安全协议证明体系

**密码学引理** 

$$
\mathbb{Z}_{p}^{*} \ni \alpha \xrightarrow{\text{ECDSA}} \sigma = (\gamma, \delta) \quad \Rightarrow \quad \text{云原生场景密钥演化}
$$

**威胁建模推论** 

$$
\forall x \in \mathcal{V},\ \exists y \mid \mathop{\mathbb{P}}(X \leq \phi(y)) \geq 0.95 \quad \Rightarrow \quad \text{APT攻击概率收敛}
$$

---

### 🌩️ 尼采安全哲学

$$
\mathop{\text{lim}}_{t\to\infty} \frac{\partial \Psi}{\partial t} = \lambda \Psi \quad \text{其中} \ \Psi = \text{"深自缄默→声震人间"}
$$

---

### 📡 安全信道协议

$$
\begin{array}{ccc} \text{CTF Writeup库} & \xleftrightarrow{\text{TLS1.3}} & \text{漏洞星球} \\ & \Updownarrow & \\ \text{云安全智库} & \xrightarrow{\text{AES-GCM}} & \text{我的GitHub} \\ \end{array}
$$

---

#### 🔗 安全握手请求

$$
\text{Let’s } \mathop{\text{Handshake}} \left( \frac{\text{你}}{\text{我}} \right) \ni \{ \text{知乎|GitHub|阿里云} \}
$$

