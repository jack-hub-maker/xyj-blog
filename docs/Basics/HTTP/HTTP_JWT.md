# 鉴权与加密算法

## session

本来 session 是一个抽象概念，开发者为了实现中断和继续等操作，将 user agent 和 server 之间一对一的交互，抽象为“会话”，进而衍生出“会话状态”，也就是 session 的概念。

而 cookie 是一个实际存在的东西，http 协议中定义在 header 中的字段。可以认为是 session 的一种后端无状态实现。

服务端执行 session 机制时候会生成 session 的 id 值，这个 id 值会发送给客户端，客户端每次请求都会把这个 id 值放到 http 请求的头部发送给服务端，而这个 id 值在客户端会保存下来，保存的容器就是 cookie，因此当我们完全禁掉浏览器的 cookie 的时候，服务端的 session 也会不能正常使用。

## Token

- session 要求**服务端存储信息**，并且根据 id 能够检索，而 token 不需要（因为信息就在 token

中，这样实现了服务端无状态化）。在大规模系统中，对每个请求都检索会话信息可能是一

个复杂和耗时的过程。但另外一方面服务端要通过 token 来解析用户身份也需要定义好相应

的协议（比如 JWT）。

- session 一般通过 cookie 来交互，而 token 方式更加灵活，可以是 cookie，也可以是

header，也可以放在请求的内容中。不使用 cookie 可以带来跨域上的便利性。

- token 的生成方式更加多样化，可以由第三方模块来提供。
- token 若被盗用，服务端无法感知，cookie 信息存储在用户自己电脑中，被盗用风险略小。

### 优点

- 可扩展性好 应用程序分布式部署的情况下，session 需要做多机数据共享，通常可以存在数据库或者 redis 里面。而 jwt 不需要。
- 无状态 jwt 不在服务端存储任何状态。RESTful API 的原则之一是无状态，发出请求时，总会返回带有参数的响应，不会产生附加影响。用户的认证状态引入这种附加影响，这破坏了这一原则。另外 jwt 的载荷中可以存储一些常用信息，用于交换信息，有效地使用 JWT，可以降低服务器查询数据库的次数。

### 缺点

- **安全性**：由于 jwt 的 payload 是使用 base64 编码的，并没有加密，因此 jwt 中不能存储敏感数据。而 session 的信息是存在服务端的，相对来说更安全。
- **jwt 太长**。所有的数据都被放到 JWT 里，如果还要进行一些数据交换，那载荷会更大，http 请求的 Header 可能比 Body 还要大。
- **一次性**。 状态无法修改，想要修改必须签发一个新的 jwt
  - 重新签发后，导致旧的 jwt 仍然能登录，但拿到的信息是过时的，这就**需要服务端进行额外的逻辑**，比如设置黑名单，签发新的后把旧的拉入黑名单
  - 续签。在 redis 中单独为每个 jwt 设置过期时间，每次访问时刷新 jwt 的过期时间。

**可以看出想要破解 jwt 一次性的特性，就需要在服务端存储 jwt 的状态。但是引入 redis 之后，就把无状态的 jwt 硬生生变成了有状态了，违背了 jwt 的初衷。而且这个方案和 session 都差不多了。**

## JWT(JSON WEB TOKEN) 原理解析

1. Bearer Token 包含三个组成部分：令牌头、payload、哈希

```JavaScript
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkYXRhIjoidGVzdCIsImV4cCI6MTU2NzY5NjEzNCwiaWF0Ij
oxNTY3NjkyNTM0fQ.OzDruSCbXFokv1zFpkv22Z_9AJGCHG5fT_WnEaf72EA
```

ey 开头表示 使用 base64 加密，可以反向解密得出临牌头和 payload

第三个 . 是使用 前两个 base64 加密算法加密得出的签名

```JavaScript
verify: key => {
            const ary = token.split('.')
            const hmac = crypto.createHmac('SHA256', key).update(ary[0] + '.' + ary[1]).digest('base64');
            return hmac === ary[2] + '='
}
```

## 加密算法

### base64 加密

#### 加密

```JavaScript
function base64Encrypt(value) {
    return new Buffer(value).toString("base64");
}
```

#### 解密

```JavaScript
function base64Decryption(value) {
    return new Buffer(value, "base64").toString();
}
```

### 内容摘要类（MD5、SHA1、SHA256、SHA512）

- 把一个不定长摘要定长结果
- 摘要 yanglaoshi -> x
- 雪崩效应
- 加密不可逆

```JavaScript
//Hex编码是以4比特作为一个单位编码，用4是因为计算机进位是2的倍数，而为了能把比特串分割开来，最适中就是取16进制；所以Hex编码就是16进制编码；
//MD5
var md5c = crypto.createHash("md5").update("加密内容ABCD1234").digest("hex");
console.log("MD5加密后结果： %s", md5c);
//sha-1 or sha - 2
var SHA1 = crypto.createHash("sha1"/* sha2 */).update("加密内容ABCD1234").digest("hex");

// SHA256加密(Hmac方式)
// HMAC是密钥相关的哈希运算消息认证码，HMAC运算利用哈希算法，以一个密钥和一个消息为输入，生成一个消息摘要作为输出。
const HMAC = crypto.createHmac('SHA256', key).update(value).digest('base64');
```

### 内容加密解密类又分为： 对称加密解密（AES），非对称加密解密(RSA)

可逆！

#### 对称加密算法 AES

```JavaScript
//AES对称加密
var secretkey = "passwd";//唯一（公共）秘钥
var content = "需要加密的内容ABC";
var cipher = crypto.createCipher('aes192', secretkey);//使用aes192加密
var enc = cipher.update(content, "utf8", "hex");//编码方式从utf-8转为hex;
enc += cipher.final('hex');//编码方式转为hex;
//
//AES对称解密
var decipher = crypto.createDecipher('aes192', secretkey);
var dec = decipher.update(enc, "hex", "utf8");
dec += decipher.final("utf8");
console.log("AES对称解密结果：" + dec);
```

#### **RSA 非对称加密**

先使用 openSSl 生成 `公钥` 和 `私钥`

```cmd
openssl genrsa -out privatekey.pem 1024
openssl rsa -in privatekey.pem -pubout -out publickey.pem
```

公钥一般用来进行加密，而私钥一般用来进行解密，当然你也可以颠倒过来使用，私钥加密公钥解密都是可以的（只是一般不这么使用）。

```JavaScript
const fs = require("fs");

const privatepem2 = fs.readFileSync("./privatekey.pem");//私有key【需要 pem 编码的key】server.pem
const publicpem2 = fs.readFileSync("./publickey.pem");//公有key【需要 pem 编码的key】cert.pem
const prikey2 = privatepem2.toString();
const pubkey2 = publicpem2.toString();
// 加密方法
var encrypt = (data, key) => {
    // 注意，第二个参数是Buffer类型
    return crypto.publicEncrypt(key, Buffer.from(data));
};
// 解密方法
var decrypt = (encrypted, key) => {
    // 注意，encrypted是Buffer类型
    return crypto.privateDecrypt(key, encrypted);
};

const plainText = "我是RSA非对称加密字符串内容";
const crypted = encrypt(plainText, pubkey2); // 加密
const decrypted = decrypt(crypted, prikey2); // 解密
console.log("RSA非对称解密结果:%s", decrypted.toString());
```

### 内容签名类（RSA+SHA1 或 RSA+SHA256 或 RSA+MD5 等等）

“信息内容签名”其实和我们日常中对纸质文件进行签名是一个道理。又称为“数字签名”，包括报文摘要。报文摘要和非对称加密一起，提供数字签名的方法。

数字签名主要是**保证信息的完整和提供信息发送者的身份认证和不可抵赖性**，这其中，“完整性”主要就是由报文摘要提供的，报文摘要就是用来防止发送的报文被篡改。

**使用流程：**

- 使用 RSA 私钥进行签名（对信息报文生成的摘要进行私钥签名）生成签名串，一般是 16 进制字符串
- 使用 RSA 公钥进行签名校验（验明正身）

JWT 中的第三段签名就是内容签名的概念，将令牌头和参数信息进行摘要后生成一个全新的签名。

实际案例可以学习 [ HTTP_https ](./HTTP_https.md)
