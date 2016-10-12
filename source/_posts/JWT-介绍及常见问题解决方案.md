---
title: JWT 介绍及常见问题解决方案
date: 2016-10-09 11:14:05
tags: JWT
---

JWT 全称Json Web Token，是一种基于 IETF 定制的开放标准 [RFC-7519](https://tools.ietf.org/html/rfc7519]https://tools.ietf.org/html/rfc7519) ，用作 **身份认证**与**数据传输** 的 Token。因其 **轻量** 与 **无状态** 的特性，被广泛用于 Api 的设计，以及解决在微服务、分布式应用中，** 用户身份认证 ** 的问题。

本文先介绍 Json Web Token ，后面在探讨 JWT 常见问题以及解决方案。

<!-- more -->

## JWT 的组成

### 头部-header

这部分包括了两个类型：

* typ: token 的类型，一般都为 JWT。
* alg: 算法的类型，主要有 HMAC-SHA256  和 RSA-SHA256。

**tip**：这里提一下两个加密算法。 HMAC-SHA-256 算法，即 HMAC 运算利用哈希算法，以一个密钥和一个消息为输入，生成一个消息摘要作为输出，因为引入了密钥，所以 HMAC-SHA-256 不在是个哈希算法，更像一个加密算法。RSA-SHA256 算法，先使用 RSA 算法加密消息，在经过 SHA256 算法来进行哈希签名。

### 载荷-Payload

承载了 Token 的具体内容，除去 JWT 提供的默认字段，用户也可以自定义字段。

* **iss**: The issuer of the token，token 是给谁的。
* **sub**: The subject of the token，token 主题。
* **exp**: Expiration Time。 token 过期时间，Unix 时间戳格式。
* **iat**: Issued At。 token 创建时间， Unix 时间戳格式。
* **jti**: JWT ID。针对当前 token 的唯一标识。

从 Payload 的结构上，我们可以看得出 JWT  保存了 **用户常用的信息，**服务端每次从用户请求中获取 Token 后就可以从里面拿到用户的基本信息，而不像 session + cookie 那套机制，服务端将用户信息保存在 session 中。由此我们可以说，JWT 是 **自说明，无状态的**。

### 签名-Signature

JWT 的加密解密过程都是在服务端完成，用户登录后成功后，每次请求都带上 Token 作为身份凭证，客户端可以读取 token 的内容，但不需要编辑它。所以，为了防止这个身份凭证被人串改，需要签名。

签名要利用 Header 中声明的加密算法，如选用了HmacSHA256 算法，则公式如下
```
$encodedContent = base64UrlEncode(header) + "." + base64UrlEncode(payload);
$signature = hashHmacSHA256($encodedContent);
```

因为有许多类库已经提供了对 JWT 的支持，所以签名和校验签名，它们都会帮我们完成。我们也可以在 http://jwt.io 上来验证我们的签名。

## Token 机制的 认证流程

* 客户端使用用户名跟密码请求登录，登陆成功服务端发放 Token

* 客户端收到 Token 以后可以把它存储起来，比如放在 Cookie 里或者 Local Storage 或者响应头Authorization里

* 客户端每次向服务端请求资源的时候，在 header 中带上 Token

* 服务端使用拦截器校验每个请求header 中的 token，如果验证成功则返回请求的数据。

## JWT 的安全性问题

注意到 JWT 中的承载用户数据的 Payload 这部分数据只是简单的经过 base64 处理，这意味着一旦有 Token 泄露，这部分数据就会被暴露。针对这一点，在生产环境中，我们都会要求全站使用 HTTPS，现如今，这应该是企业站点的标配了，可以很大程度上减少 Token 被盗。其次，太过隐私的东西，不要放在 Payload。另外，我们可以使用 JSON Web Encryption（JWE）来对 Payload 中部分数据，乃至整个 Payload 进行对称加密，不过这回导致额外的开销。
 
## JWT 的优点

* JWT 是无状态的，对 resetful 接口与架构支持非常友好，这使得它更适用于 app 的开发，因为不需要再去把 cookie 那一套东西搬过来；也更加方便集群拓展，因为我们不需要再去搭建和维护一个会话集群。

* 天生支持跨域访问，因为 JWT 把用户的基本信息存储在 HTTP header 中，如果使用 cookie，则还需要使用一些跨域的解决方案。

* 天然防 CSRF 攻击。CSRF 攻击成功的原因正是黑客利用了 cookie 中存储的有效认证信息，用户在不知情的情况下，在别的网站点击了了它精心构造的链接，由此 cookie 在用户不知情的情况下被利用了。

* 基于现有的标准，许多语言都提供了对 JWT 支持的包，一些框架也提供了对 JWT 的整合，入手也很简单。

* 性能更优，因为不使用 cookie + session  作为认证的机制， 所有服务器端不需要在每次会话对 cookie 和 session  进行序列与反序列化，我们需要做的就是使用选定的算法和私钥解密令牌，获取用户信息。

## JWT 常见问题及解决方案

### 如何使 token 失效？

在这个问题在 [stackoverflow](http://stackoverflow.com/questions/21978658/invalidating-json-web-tokens) 上有讨论，在此我总结了下，token 需要失效的场景以及解决方案：

|应用场景|解决方案|
|---|---|
|用户退出登陆，但是 token 尚未过期|客户端移除 token，服务端使用 redis 维护一份 token 黑名单，并设置超时时间为其剩余的有效时间|
|用户在登陆状态下，修改了密码|服务端发放新 token ，旧 token 放入redis 黑名单，设置超时时间|
|admin 禁用了账户，不允许账户登陆|将用户放入黑名单|
|admin 修改了 token 中某些信息，比如用户角色|服务端发放新 token ，旧 token 放入redis 黑名单，设置超时时间。|
|某些特殊情况下需要禁止某个用户的登陆，比如发现 token 被黑客盗取。|如果是单个用户，可以暂时禁用账户，如果是多个，考虑修改加密的key，注意这会让全部 token 失效|

**tip**: 使用 redis 来维护 token  黑名单，这里存储在 redis 的数据实体可以自己定义，以下是我的设计:
```
{
    "key": "userId",
    "value": {
        "invalid_type": "失效类型:退出登陆、修改密码、修改角色、账号禁用",
        "start_time": " token 的 发放时间 iat  < start_time 都视为非法",
        "exp": "redis 存储时间"
    }
}
```
    
### 如何实现 token 自动刷新?

因为 token 设置了一个有效期，所以需要引入一套刷新机制，避免让用户重新输入密码登陆而带来的体验问题。和上面的问题一样，这个问题在  [stackoverflow](http://stackoverflow.com/questions/26739167/jwt-json-web-token-automatic-prolongation-of-expiration) 也有讨论。综合笔者查阅的资料，总结如下：

这个问题要具体情况具体分析：

#### 针对手机 App 应用

对于 App 这种需要安装在手机上的应用，因为一般情况下一个手机对应一个用户，所以很多 App 一般都只输入一次密码，对此有以下几种解决方案：

* 客户端保存密码自动登陆方案。客户端获取用户密码后，将密码对称加密保存在客户端，Token 失效后，客户端在将密码取出帮用户自动登陆，从而获取新的 token。这种方案适合 app ，保存密码在本地这种方案要重点解决 密钥和用户密码的安全。

* 使用"永久"有效的 refresh token 方案。永久的意思是指在除去用户修改密码这种特性情况外有效。当然这个 refresh token  要和用户设备的信息挂钩，要确保只能在一个设备上使用。服务端也要对这个 refresh token 有所控制，即要做到随时可以时这个 refresh token 失效。因此，这个 refresh token 常被设计为一个包含了用户的设备信息、以及服务端存储的一个随机数。

#### 针对 Web 应用

对于 Web 应用，因为依托于浏览器，这里在考虑 Token 自动刷新时，更多借鉴与 session 的机制。

* 定时校验刷新 Token 方案。具体思路为，将 Token 过期时间设置长一点，比如3个小时；然后每过一定时间段，比如15分钟，然后客户端通过一个全局的拦截器，在每次请求 Api 的时候，计算距离超时时间是否又缩短了一个小时，是的话就向服务端申请新的 token ，新 token 有效时间延长了一个小时。这种方案相对灵活，适合 Web App。

* 使用一个有较长期限的 refresh token 方案。这个 refresh token  相对于普通的 token 只是有效时间长一些。它的设计相对麻烦，因为他同要解决 refresh token 自刷新问题，服务端可控制失效问题，这里可以借鉴 Oauth 2.0 中的refresh token 机制。

## JWT 的不足

JWT 设计的初衷是轻量、无状态，这是它的优点，但也是它的不足之处(事物都有两面性)，因为它大小有限制，另外无状态意味着它不能即使的判断 Token 的有效性，在需要 token 过期时要引入黑名单机制。

**tip**:其实有种做法是在数据库里面存一些字段，然后每次校验 token 时候都去查询数据库，这种做法可以做到即时校验 token 的有效性，但是这种做法开销较大，笔者并不推荐。也有人说这种做法违背了 JWT 无状态的初衷，笔者的理解是，每次的数据库查询，硬是把 token 变成了带状态的。

## JWT 存储(浏览器)

客户端获取 Token 后， 需要临时存储，目前可以选择 local storage、session storage 和 cookie。注意如果 token 存放在 Web Storage (local storage、session storage)，需要留意 XSS 注入，因为 js 可以访问到这里。如果放入 cookie 的话，要记得加上 readOnyl，不过这又要留意 CSRF 攻击了。

## 参考

1、[RFC7519 协议](https://tools.ietf.org/html/rfc7519)

2、[stackoverflow 上关于如何使 Token 失效的探讨](http://stackoverflow.com/questions/21978658/invalidating-json-web-tokens#)

3、[stackoverflow 上关于 Token 到期自动延长的解决方案的探讨](http://stackoverflow.com/questions/26739167/jwt-json-web-token-automatic-prolongation-of-expiration)

4、[auth0 中永久refresh 的设计](https://auth0.com/docs/tokens/refresh-token#revoke-a-refresh-token)

5、[wiki 上关于无状态定义的描述](https://en.wikipedia.org/wiki/Stateless_protocol)

6、[cookies Vs token](https://auth0.com/blog/angularjs-authentication-with-cookies-vs-token/)

7、[refresh token 使用](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)

8、[Where to Store your JWTs – Cookies vs HTML5 Web Storage](https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage)