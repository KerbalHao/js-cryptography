# JavaScript 是如何工作的：加密 + 如何处理中间人攻击
> 本文翻译自 (Ukpaiugochi)[https://medium.com/@ukpaiugochi0?source=post_page-----bf8fc6be546c--------------------------------]的
(How JavaScript works: cryptography + how to deal with man-in-the-middle (MITM) attacks)[https://blog.sessionstack.com/how-javascript-works-cryptography-how-to-deal-with-man-in-the-middle-mitm-attacks-bf8fc6be546c]

网络安全是 IT 领域的一个重要领域。全球有数不清的人每天通过互联网相互交流。正因如此，你的信息在对应的第三方获取之前，可能会呗监听或者劫持。同样，你的个人信息也有可能被利用计算机网络漏洞的黑客盗取。

那么人们如何安全地在互联网发送信息呢？JavaScript 在这一过程中扮演着什么样的角色呢？这篇文章将为您揭晓答案。

在这篇文章中，你将会了解到什么是加密，它是如何在 JavaScript 中工作的，它又是如何解决中间攻击的。

## 加密(Cryptography)的介绍
加密是保护信息和通讯只能由发送者和目标接收者访问的处理。加密使用多种技术来正确地保护通讯。这些技术可以包括使用密码器和解密器的加密和解密，使用各种算法对通信过程进行散列，或签名生成和验证。

因为很多人使用由 JavaScript 构建的移动应用在网上交流，了解 JavaScript 加密也是很有必要的。在下一节中，我们将会看到 JavaScript 的 Web Cryptography API 和它是如何支持加密的。

## JavaScript 的 Web Cryptography API
出于确保网络通信安全的重要性，有一些网络浏览器已经实现了加密接口。可是，这些接口定义不明确或者听起来不那么安全。JavaScript 的 Web Cryptography API 提供了一个定义明确的接口，叫 (SubtleCrypto)[https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto]

在不引入第三方库的情况下，JavaScript 的 Web Cryptography API 允许开发者将基础的加密函数并入他们的应用中，你可以签署文档，执行验证，执行通信的完整新检测。

例如，你可以运行以下代码，获得8位无符号整型数组格式的安全加密的随机数据：
```js
const secure = window.crypto.getRandomValues(new Uint8Array(10));
console.log(secure);
```

你可以在自己的网页控制台运行代码。例如，我在 Chrome 的控制台运行这段代码，我将会获得 10 个 8-bit 的随机生成地无符号数字。
![0_QbI-hozmSVibF6DW.png](https://pic.gksec.com/2021/05/28/64923b5ba1375/0_qbi-hozmsvibf6dw.png)

让我们来看一下 JavaScript 的  web cryptography API 是如何工作的，而我们又是如何在我们的浏览器控制台实现的。

通过合适的 JavaScript 的 web cryptography API，服务器无法看到被加密的数据。只有发送者和接收者可以访问到通信数据
![0_EoQo41lZY6_RXAii.png](https://pic.gksec.com/2021/05/28/fc08946d80ef4/0_eoqo41lzy6_rxaii.png)

从上图中，你可以看到发送者的数据通过 API 被加密了。接收者使用密钥(key)来解锁数据，服务器和数据库无法破译被加密的数据。你可以执行基本的加密操作，比如哈希，签名生成和验证、加密和解密，这些将在本文中进一步讨论。

## 基本的加密函数
有许多加密函数你可以用 JavaScript 的  web cryptography API 来执行。在本节中，我们将关注基本的加密函数，如哈希、签名生成和验证、加密和解密。