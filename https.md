# https
* 窃听

  加密
    * 对称加密：效率高，对会话内容进行加密；靠自身无法安全进行密钥交换。
    * 非对称加密：效率低，不参与会话内容的加密，可以安全地交换对称密钥，前提是客户端拿到真实的服务端公钥。

* 篡改

  签名
    * CA对服务端公钥进行摘要，CA私钥对摘要进行加密；客户端内置的CA公钥解密得到摘要1，客户端用同样的算法对服务端公钥进行摘要得到摘要2，比对摘要1和摘要2。

* 冒充

  CA，权威机构

***

# 数字摘要，数字签名，数字证书
* 网站信息
  * 域名
  * 公司名
  * 服务器公钥

* CA给证书添加的明文信息
  * CA机构信息
  * 证书有效期
  * 签名算法

* 数字摘要
  * 哈希算法对`网站信息`+`CA给证书添加的明文信息`得到的哈希值

* 数字签名
  * CA私钥对`数字摘要`进行加密得到的密文

* 数字证书
  * `网站信息`+`CA给证书添加的明文信息`+`数字签名`

***

# https RSA握手过程

1. 客户端发送`client hello`，包含`客户端随机数`等。

2. 服务端回复`server hello`+`服务器数字证书`+`server hello done`，`server hello`包含`服务端随机数`等。

3. 客户端验证`服务器数字证书`，若通过则拿到真实的`服务端公钥`；

   再生成一个随机数`pre-master`，用`服务端公钥`对`pre-master`进行加密；
   
   用`pre-master`+`客户端随机数`+`服务端随机数`生成对称的`会话密钥`；
   
   对发送的握手信息进行摘要，用`会话密钥`对摘要进行加密得到`客户端Encrypted Handshake Message`；

   客户端发送`pre-master密文`+`客户端Encrypted Handshake Message`。

4. 服务端用`服务端私钥`对`pre-master密文`解密，得到`pre-master`后+`客户端随机数`+`服务端随机数`生成与客户端一致的`会话密钥`；

   用`会话密钥`对`客户端Encrypted Handshake Message`解密得到摘要1，对接收到的`客户端握手信息`进行摘要2，验证两者的摘要值；

   对发送的握手信息进行摘要，用`会话密钥`对摘要进行加密得到`服务端Encrypted Handshake Message`；
   
   服务端回复`服务端Encrypted Handshake Message`。

* 为什么要生成`客户端随机数`和`服务端随机数`?
  * 为了更加随机。

## 客户端如何验证`服务器数字证书`

* 客户端拿到`服务器数字证书`，如果其中不包含`中间CA证书`，则向记录在`服务器数字证书`的`中间CA`请求`中间CA证书`；三层证书链的`中间CA证书`是由某个`根CA`签发，客户端在操作系统和浏览器查看有无这个`根CA证书`，若有的话，就用`根CA证书`中的`根CA公钥`对`中间CA证书`中的`数字签名`解密得到摘要值1，再用`中间CA证书`中的`摘要算法`对`中间CA证书`的明文信息进行哈希得到摘要值2，比对两者摘要值，若通过则证明`中间CA公钥`是可靠的；下一步就是用同样的方法和`中间CA公钥`验证`服务器数字证书`中的`服务器公钥`是可靠的。

* 总结的话，就是根CA公钥验证中间CA公钥，中间CA公钥验证服务器公钥。

* 为何`根CA`不直接对服务器直接签名？
  * 因为`根CA私钥`泄露的话，影响范围更大，不经常出面签发的泄露风险就更低。

* 为何`CA私钥`不直接对原始信息加密，而是对原始信息哈希后的摘要值加密？
  * 因为非对称密钥的效率比较低，摘要值比原始信息内容小很多。

# 防止中间人攻击

* 中间人可以拿到`客户端随机数`+`服务端随机数`+`pre-master密文`，由于没有`服务端私钥`，就无法得到`pre-master`,也就无法生成`会话密钥`。

* 伪造`服务端公钥`，客户端就会在第3步验证`服务器数字证书`失败。

* 伪造`随机数`或者`Encrypted Handshake Message`，服务端就会在第3步验证时失败，中断连接。

# https ECDHE握手过程

1. 客户端发送`client hello`，包含`客户端随机数`等。

2. 服务端生成`临时服务端非对称密钥`，用`服务端私钥0`对`临时服务端公钥`的摘要值加密；

   回复`server hello`+`服务器数字证书`+`server Key exchange`+`server hello done`，`server hello`包含`服务端随机数`等；
   
   `server Key exchange`包含`临时服务端公钥`和它的摘要值密文。

3. 客户端验证`服务器数字证书`，若通过则拿到真实的`服务端公钥0`；再用`服务端公钥0`验证`临时服务端公钥`的真实性。
   
   客户端生成`临时客户端非对称密钥`，利用`临时客户端私钥`+`临时服务端公钥`等计算出`x值`，`x值`+`客户端随机数`+`服务端随机数`生成最终的`会话密钥`；
   
   客户端发送`client key exchange`+`客户端Encrypted Handshake Message`。

   `client key exchange`包含`临时客户端公钥`。

4. 服务端利用`临时服务端私钥`+`临时客户端公钥`等计算出`x值`，`x值`+`客户端随机数`+`服务端随机数`生成最终的`会话密钥`；

   服务端回复`服务端Encrypted Handshake Message`。

* 为什么第3步的`client key exchange`没有包含摘要值密文？

  * 因为签名和验签需要对方有真实的公钥，而服务端此时没有持有与客户端私钥成对的公钥。如果客户端用`服务端公钥0`签名，中间人同样持有`服务端公钥0`，就可以伪造`临时客户端公钥`。既然防止不了伪造，就没必要签名了。
