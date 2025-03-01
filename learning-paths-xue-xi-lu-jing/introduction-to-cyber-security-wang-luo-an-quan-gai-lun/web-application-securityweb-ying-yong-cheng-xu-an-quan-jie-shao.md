---
icon: check
description: 本文相关内容：了解 Web 应用程序并探索它们的一些常见安全问题
cover: ../../.gitbook/assets/intro-to-offensive-security.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Web Application Security(Web应用程序安全介绍)

TryHackMe实验房间链接：[https://tryhackme.com/room/introwebapplicationsecurity](https://tryhackme.com/room/introwebapplicationsecurity)

## Web应用程序简介

我们每个人都会在我们的计算机上使用不同的程序。 一般来说，当程序在计算机上运行时，就将使用计算机的处理能力和存储功能，而且，要使用一个程序，我们还需要先安装它。如果我们想不经过安装操作就直接使用程序应该怎么办？

<figure><img src="../../.gitbook/assets/image-20230421124001893.png" alt=""><figcaption></figcaption></figure>

Web 应用程序类似于普通的“程序”，并且只要我们的计算机上有一个现代的、标准的 Web 浏览器，例如 Firefox、Safari 或 Chrome等，我们就可以不经安装而直接使用Web应用程序——当我们想运行Web应用程序时，我们无需安装 运行时所需的每个程序，只需直接访问相关浏览器页面即可。

以下是关于 Web 应用程序的一些示例：

* 网络邮件，例如 Tutanota、Protonmail、Outlook 和 Gmail
* 在线办公套件，如 Microsoft Office 365（Word、Excel 和 PowerPoint）、Google Drive（Docs、Sheets 和 Slides）和 Zoho Office（Writer、Sheet 和 Show）
* 在线购物网站，例如 Amazon.com、AliExpress 和 Etsy

其他示例还包括网上银行、汇款、天气预报和网络社交媒体等，成千上万的Web应用程序提供了数不胜数的在线服务。

<figure><img src="../../.gitbook/assets/image-20230421124018938.png" alt=""><figcaption></figcaption></figure>

Web 应用程序的概念是一个在远程服务器上运行的程序，而服务器是指连续运行以“服务”用户客户端的计算机系统，在这种情况下，服务器将运行特定类型的程序，并且这些程序可以通过Web浏览器被访问。

以一个在线购物 Web 应用程序为例，此 Web 应用程序将从数据库服务器中读取有关产品及其详细信息的数据，数据库将用于 以有组织的方式存储信息，在本例中这些数据可能会包括有关产品、客户和发票的信息。（数据库服务器会负责许多功能，包括对数据库进行读取、搜索和写入等操作）

一个在线购物 Web 应用程序在运行时 可能需要访问多个数据库，例如：

* 产品数据库：该数据库包含有关产品的详细信息，例如名称、图像、规格和价格。
* 客户数据库：该数据库包含与客户相关的所有详细信息，例如姓名、地址、电子邮件和电话号码。
* 销售数据库：我们希望在这个数据库中看到每个客户购买了什么以及他们是如何支付的。

我们已经大概了解了存储在任何在线购物系统中的信息量，假如攻击者设法利用（破解）Web 应用程序并窃取客户的数据库内容，那么这将给公司及相关客户带来重大损失。

下图显示了在Online购物网站上搜索商品可能将发生什么，在最简单的Online购物网站中，搜索商品可能会经历以下四个步骤：

1. 用户在搜索字段中输入项目名称或相关关键字，Web 浏览器会将被搜索的关键字发送给在线购物 Web 应用程序。
2. Web 应用程序查询（搜索）产品数据库以查找用户所提交的关键字。
3. 产品数据库将与用户提供的关键字相匹配的搜索结果返回给 Web 应用程序。
4. Web 应用程序将结果格式化为友好可读的网页页面并将它们返回给用户以供浏览。

<figure><img src="../../.gitbook/assets/image-20230319224615610.png" alt=""><figcaption></figcaption></figure>

从用户的角度来看，他们只会访问到一个隐藏所有技术基础设施的在线商店页面。

<figure><img src="../../.gitbook/assets/image-20230319224808863.png" alt=""><figcaption></figcaption></figure>

### **答题**

<figure><img src="../../.gitbook/assets/image-20230319213257215.png" alt=""><figcaption></figcaption></figure>

> Browser - 浏览器

## Web 应用程序常见安全风险

假设你想从网上商店购买商品，你就会希望能够在相关 Web 应用程序上执行某些功能，用户完成一个在线商品订单可能分为以下几个步骤：

<figure><img src="../../.gitbook/assets/image-20230319225857754.png" alt=""><figcaption></figcaption></figure>

针对 Web 应用程序的常见攻击有很多，下面简单介绍一些攻击方式：

* 在登录网站时：攻击者可以通过进行多次登录尝试来枚举发现有效密码；攻击者能够使用密码字典和一些自动化工具来针对登录页面来进行多次测试。(暴力枚举)
* 在搜索产品时：攻击者可以通过在搜索字段中添加特定字符和代码来尝试破坏目标系统；攻击者的目的是让目标系统返回它不应该返回的数据或者执行它不应该执行的程序。(注入类攻击)
* 在提供付款细节时：攻击者会检查 和付款细节相关的数据 是以明文形式发送还是使用了弱加密；加密是指 让数据在不知道密钥或密码的情况下变得不可读。(明文、弱加密)

tips：本小节只是介绍一些针对Web应用程序的攻击方式，而并非全部。

### 身份识别和验证错误

身份识别(Identification)是指 可以对用户身份进行唯一识别的能力，而身份验证(authentication)是指 可以证明用户实际身份和用户所声称的身份相符合的能力；当用户登录网上商店时，正常情况下：商店必须先识别用户的身份并进行身份验证，然后才能让用户使用该在线购物系统。

如果网站的身份识别和身份验证机制不佳，那么在网站登录界面就可能存在一些漏洞，关于此类漏洞的示例有：

* 目标网站允许攻击者使用暴力枚举；攻击者能够尝试多次输入不同密码以进行登录操作——通常攻击者会使用自动化工具来暴力枚举，从而找到有效的登录凭据。
* 目标网站允许正常用户使用弱密码；弱密码通常很容易被攻击者猜到。
* 目标网站以明文形式存储用户密码；如果此时攻击者能够设法读取包含密码的文件，那么他们就能够获知存储的密码内容。

<figure><img src="../../.gitbook/assets/image-20230319234328832.png" alt=""><figcaption></figcaption></figure>

### 损坏的访问控制机制

访问控制(Access control)机制 能够确保每个用户只能访问与其角色或工作相关的文件（文档、图像等）；例如，公司的网站管理员应该不希望市场部的某人能够直接访问（阅读）财务部的文档。

与访问控制相关的示例漏洞包括：

* 网站管理员未能应用最小权限原则并为用户提供超出其需要的访问权限；例如，在线客户应该能够查看商品的价格，但他们不应该能够更改商品价格。
* 攻击者能够使用用户的唯一标识符查看或修改他人的帐户信息；例如，你应该不希望一个银行客户能够查看另一个客户的交易详情。
* 攻击者能够以未经身份验证的用户身份 浏览需要身份验证（登录）的页面；例如，我们不应该让任何人在登录之前就能查看到网络邮件内容。

### 注入攻击

注入攻击是指 对 Web 应用程序中的注入类漏洞进行利用，在注入类漏洞存在的情况下：攻击者可以插入恶意代码作为用户输入内容的一部分。

注入类漏洞存在的一个原因是 目标网站缺乏对“用户输入”的适当验证和清理。

### 未成功加密

此类漏洞的存在是由于发生了与密码学相关的错误，密码学侧重于数据的加密和解密过程，加密是指 对明文内容使用加密算法计算以得到密文；对于没有密钥解密的人来说，经过加密产生的密文是无法读懂的，换句话说，加密能够确保没有人可以在不知道密钥的情况下直接读取数据，只有使用密钥进行解密 才能将密文转换回原始明文。

未成功加密(加密失败)的例子包括：

* 目标网站以明文形式发送敏感数据，例如，网站使用 HTTP 协议而不是 HTTPS 协议； HTTP 是用于访问 Web 的协议，而 HTTPS 是 HTTP 的安全版本，攻击者可以阅读通过 HTTP 协议发送的所有内容，但不能阅读使用 HTTPS 协议发送的内容。
* 目标网站依赖弱密码算法进行加密（很容易被破解）；有一种古老的密码算法是将每个字母移位一个，例如，将“TRY HACK ME”变成“USZ IBDL NF”，如果使用这种加密算法就很容易被攻击者破解密文。
* 目标网站使用默认或弱密钥进行加密；破解使用 1234 作为密钥的加密并不困难，所以如果使用弱密钥加密就很容易被攻击者破解。

<figure><img src="../../.gitbook/assets/image-20230421124046841.png" alt=""><figcaption></figcaption></figure>

### **答题**

<figure><img src="../../.gitbook/assets/image-20230319214337998.png" alt=""><figcaption></figcaption></figure>

> Identification and Authentication Failure 身份识别和身份验证错误
>
> Cryptographic Failures 未成功加密（加密失败）

## Web 应用程序安全实例

本小节将调查一个存在不安全直接对象引用 (IDOR漏洞) 的易受攻击的示例网站。

IDOR漏洞属于损坏的访问控制机制的一种，所谓 损坏的访问控制机制 意味着攻击者可以访问不适合他们访问的信息 或者 执行不适合他们执行的操作。

Web 服务器能够接收用户提供的输入以检索对象（文件、数据、文档），这些被检索的对象可能是按顺序编号的；我们假设当前用户有权访问名为 IMG\_1003.JPG 的照片，我们可能会猜测 Web服务器上还存在IMG\_1002.JPG照片 和 IMG\_1004.JPG照片；然而，即使我们已经知道了图像文件的名称，Web 应用程序也不应该向我们提供某些图像。一般来说，如果Web应用程序对“用户输入”的信任度过高，就可能会出现IDOR漏洞，换句话说，此时 Web 应用程序并没有严格验证用户是否有权访问所请求的对象。

即使知道了某个用户或某个产品相关的正确 URL 也不一定意味着当前用户应该能够访问这些 URL（所以这需要我们进行实际的访问验证）。假设我们已经知道某个产品页面的URL： https://store.tryhackme.thm/products/product?id=52 ；我们可以期望此 URL 会提供有关编号为 52 的产品详细信息；如果在数据库中，产品数据项是将按顺序分配编号的，那么攻击者就可以尝试使用其他数字 如 51 或 53来取代URL中的 52；一旦目标 Web 应用程序存在IDOR漏洞，那么攻击者就可能会访问到 其他已停用产品或未发布产品的相关页面。

让我们考虑一个更关键的例子(此例基于目标系统存在IDOR漏洞的前提条件下)，假设有一个URL https://store.tryhackme.thm/customers/user?id=16 ，这个URL将返回 id=16 的用户相关页面；同样，我们希望用户所对应的 ID 号是连续的，那么攻击者就可以尝试在URL中使用其他ID号 并可能成功访问到其他用户帐户所相关的页面（进而能够查看到其他用户的敏感信息）。

IDOR漏洞也适用于访问一些按顺序命名并存储的文件；例如，攻击者在URL中看到007.txt，那么可以尝试使用其他文件名称，如001.txt、006.txt等。(此例基于目标系统存在IDOR漏洞的前提条件下)

### **答题**

_在本文相关的Tryhackme实验房间页面 部署虚拟实验环境，并完成本小节对应的实例。_

<figure><img src="../../.gitbook/assets/image-20230319215632445.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image-20230319220131634.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image-20230319220240818.png" alt=""><figcaption></figcaption></figure>

> 最后得到的flag内容为：THM{IDOR\_EXPLORED} 。

<figure><img src="../../.gitbook/assets/image-20230319215450568.png" alt=""><figcaption></figcaption></figure>
