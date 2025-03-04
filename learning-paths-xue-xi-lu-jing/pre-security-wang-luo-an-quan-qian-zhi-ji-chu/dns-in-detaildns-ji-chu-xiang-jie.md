---
icon: check
description: 本文相关内容：了解DNS协议是如何工作的，以及DNS如何帮助我们访问互联网服务。
cover: ../../.gitbook/assets/f54f3b9acec93f9cdbf2f1811dff1e70.png
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

# DNS in detail(DNS基础详解)

TryHackMe实验房间链接：[https://tryhackme.com/room/dnsindetail](https://tryhackme.com/room/dnsindetail)

## 什么是DNS？

DNS 指 Domain Name System，即域名系统，DNS能为我们提供一种简单的方式来与互联网上的设备进行通信，而不需要记住复杂的IP地址。

就像每个家庭都有一个唯一的地址能够用来直接发送和接收邮件一样，互联网上的每台计算机也都有自己唯一的地址能够用于通信，这个地址被称为IP地址。

IP地址的格式如下：104.26.10.229，主要由4组0 \~ 255的二进制数字组成，中间用英文句点号隔开。当你想用浏览器来访问一个网站时，选择记住和目标网站相对应的那组复杂的IP地址 可能并不太方便，这就是DNS可以帮助我们的地方。在使用了DNS协议之后，我们就不需要记住类似于 104.26.10.229 的IP地址，而是可以选择记住和IP地址对应的域名如：tryhackme.com 。

<figure><img src="../../.gitbook/assets/image-20240208203933301.png" alt=""><figcaption></figcaption></figure>

### **答题**

阅读本小节内容并回答以下问题。

<figure><img src="../../.gitbook/assets/image-20230327231521741.png" alt=""><figcaption></figcaption></figure>

## 域层次结构

**Domain Hierarchy示意图**

<figure><img src="../../.gitbook/assets/image-20240208204034704.png" alt=""><figcaption></figcaption></figure>

**Top-Level Domain(TLD-顶级域)**

TLD指的是域名最右边的部分，例如，tryhackme.com的顶级域名是.com；TLD有两种类型，包括gTLD(通用顶级域)和ccTLD(国家代码顶级域)。

在计算机历史上，通用顶级域名主要用于告诉用户“域名的用途”，例如，.com用于商业目的，.org用于组织，.edu用于教育，.gov用于政府；而ccTLD的用途是表示地理位置，例如.ca表示位于加拿大的网站，.co.uk表示位于英国的网站等等。基于用户需求，新的通用顶级域名在大量涌入，包括.online，.club，.website，.biz等等。

要查看超过2000个顶级域名的完整列表，请访问以下链接：

https://data.iana.org/TLD/tlds-alpha-by-domain.txt

_tips：gTLD - Generic Top Level、ccTLD - Country Code Top Level Domain 。_

**Second-Level Domain(二级域)**

以tryhackme.com为例，.com部分是TLD，tryhackme部分则是二级域名。在进行域名注册时，二级域名被限制最长可使用63个字符+顶级域名(TLD)，在具体的二级域名部分只能使用"`a-z`"、"`0-9`"和连字符"`-`"(不能以连字符开头或结尾，也不能使用连续的连字符)。

**Subdomain(子域)**

在一个完整的域名中，子域位于二级域的左侧，两者之间将使用英文句点分隔，例如，在一个域名 admin.tryhackme.com 中，admin部分就是子域。

子域名的创建限制与二级域名相同，都被限制为最长可用63个字符，并且同样只能使用A -z 0-9和连字符(不能以连字符开头或结尾或使用连续连字符)。

在一个域名中，我们可以同时使用多个子域，只要用英文句点分隔即可，由此我们便能创建一个较长的域名，例如 jupiter.servers.tryhackme.com ，但是域名的整体长度必须控制在253个字符以内，也就是说：在域名整体长度没超过限制的情况下，我们可以为一个域名创建多个子域名且无数量限制。

### **答题**

阅读本小节内容并回答以下问题。

<figure><img src="../../.gitbook/assets/image-20230328001000917.png" alt=""><figcaption></figcaption></figure>

## DNS记录类型

我们已经知道DNS可用于寻找目标网站，此外，还存在多种类型的DNS记录，接下来我们将介绍一些常见的DNS记录类型。

**A 记录**

此类DNS记录将被解析为IPv4地址，例如：`104.26.10.229`

**AAAA 记录**

此类DNS记录将被解析为IPv6地址，例如：`2606:4700:20::681a:be5`

**CNAME 记录**

此类DNS记录将被解析到另一个域名，例如，TryHackMe的在线商店有一个子域名store.tryhackme.com，它能返回一个CNAME记录shops.shopify.com，然后再向shops.shopify.com发出另一个DNS请求，就能计算出相关的IP地址。

**MX 记录**

此类DNS记录将被解析为 处理你正在查询的域的电子邮件服务器地址，例如 tryhackme.com 的MX记录响应可能为 alt1.aspmx.l.google.com ；这类记录还会带有一个优先级值，此值将告诉客户端以什么顺序来尝试访问电子邮件服务器，如果主电子邮件服务器宕机，则可以将电子邮件发送到备份电子邮件服务器。

**TXT 记录**

TXT记录是自由文本字段，任何基于文本的数据都可以存储在其中。TXT记录有多种用途，一些常见的用途是 列出有权代表域发送电子邮件的服务器(这可以帮助对抗垃圾邮件和钓鱼邮件)，TXT记录还可用于 在注册第三方服务时验证域名的所有权。

### **答题**

阅读本小节内容并回答以下问题。

<figure><img src="../../.gitbook/assets/image-20230328003618226.png" alt=""><figcaption></figcaption></figure>

## 发出DNS请求

**当你发出DNS请求时会发生什么？**

1. 当你请求一个域名时，你的计算机首先会检查它自身的本地缓存，看看你最近是否查询过这个地址(即查看缓存中是否已经为目标网站存储了一个相关的 IP 地址)，如果发现本地缓存中无相关记录，你的计算机则将向递归(_**recursive**_)DNS服务器发送DNS请求。
2. 递归DNS服务器通常会由你的Internet 服务提供商(即ISP，在中国是移动、联通等)提供，但你也可以选择一些其他的递归DNS服务器。递归DNS服务器会带有一个关于“最近查找过的域名”的本地缓存，如果在此缓存中找到结果，则相关的信息将被发送回你的计算机，你的DNS请求也会在这里结束(这种域名请求情况对于谷歌、Facebook、Twitter等受欢迎以及域名请求频率很高的网站服务来说很常见)。如果在递归DNS服务器的本地缓存中无法找到域名请求的结果，接下来就会开始从互联网的根DNS服务器尝试寻找关于域名请求的正确答案。
3. 根DNS服务器是互联网的DNS主干，它们的工作是将域名请求重定向到正确的顶级域服务器，重定向的结果取决于你的域名请求内容：例如，如果你请求www.tryhackme.com，那么根DNS服务器将识别到.com的顶级域名，并会将你指向处理.com地址的正确TLD(顶级域)服务器。
4. TLD服务器会保存“在哪里能找到响应DNS请求的权威服务器”的记录。权威服务器通常也被称为域的名称服务器，例如www.tryhackme.com 的名称服务器是`kip.ns.cloudflare.com`和`uma.ns.cloudflare.com`，一个域名可能会有多个域名服务器，这是为了形成备份以防宕机。（简而言之：当 TLD 服务器收到我们的域名请求时，TLD服务器会将域名请求信息传递给适当的权威名称服务器，而权威名称服务器主要用于直接存储域的 DNS 记录）
5. 权威DNS服务器是负责存储特定域名的DNS记录的服务器，并且能对所存储的DNS记录进行及时更新。基于DNS记录的不同类型会有多条DNS记录内容，而这些与你的域名请求相关的DNS记录都存储在权威DNS服务器中，当域名请求到达权威DNS服务器之后，权威DNS服务器会将这些与你的域名请求相关的DNS记录 发送回递归DNS服务器，递归DNS服务器将会为这些DNS记录缓存一个本地副本 以备将来的请求所需，然后这些DNS记录将被转发回 发出域名请求的原始客户端机器。

<figure><img src="../../.gitbook/assets/image-20240208204158245.png" alt=""><figcaption></figcaption></figure>

Tips：每个DNS记录都会带有一个TTL（Time To Live-生存时间）值，这个值是一个以秒表示的数字，所有不超过TTL时间的DNS记录都会持续存储在计算机的本地缓存中，如果本地缓存中的DNS记录过期，那么在下次请求域名时，你可能需要再次获取相关的DNS记录（这将重复上述过程）。通过使用计算机本地缓存中的DNS记录——可以节省每次与目标服务器进行通信时 所消耗的DNS请求响应时间。

### **答题**

阅读本小节内容并回答以下问题。

<figure><img src="../../.gitbook/assets/image-20230328003958971.png" alt=""><figcaption></figcaption></figure>

## 简单示例

在和本文相关的TryHackMe实验房间中部署实验环境并回答问题。

_tips：在模拟界面构建请求以进行DNS查询并查看结果。_

查询 shop.website.thm 的CNAME记录：`nslookup --type=CNAME shop.website.thm`

<figure><img src="../../.gitbook/assets/image-20230327232336558.png" alt=""><figcaption></figcaption></figure>

> shop.website.thm 的CNAME记录是：shops.myshopify.com 。

查询 website.thm 的TXT记录：`nslookup --type=TXT website.thm`

<figure><img src="../../.gitbook/assets/image-20230327232618617.png" alt=""><figcaption></figcaption></figure>

> website.thm 的TXT记录是：THM{7012BBA60997F35A9516C2E16D2944FF} 。

查看 website.thm 的MX记录的数字优先级值：`nslookup --type=MX website.thm`

<figure><img src="../../.gitbook/assets/image-20230327232832550.png" alt=""><figcaption></figcaption></figure>

> website.thm 的MX记录的数字优先级值为：30 。

查看 website.thm 的A记录的IP地址：`nslookup --type=A website.thm`

<figure><img src="../../.gitbook/assets/image-20230327233114182.png" alt=""><figcaption></figcaption></figure>

> website.thm 的A记录的IP地址为：10.10.10.10 。

<figure><img src="../../.gitbook/assets/image-20230327233202973.png" alt=""><figcaption></figcaption></figure>
