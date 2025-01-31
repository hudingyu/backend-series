<!--
 * @Author: your name
 * @Date: 2020-04-06 17:54:50
 * @LastEditTime: 2020-04-14 15:15:46
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /backend-series/网络/HTTP.md
 -->

#### HTTP和HTTPS有什么区别？
- 端口不同：HTTP使用的是80端口，HTTPS使用443端口；
- HTTP（超文本传输协议）信息是明文传输，HTTPS运行在SSL(Secure Socket Layer)之上，添加了加密和认证机制，更加安全；
- HTTPS由于加密解密会带来更大的CPU和内存开销；
- HTTPS通信需要证书，一般需要向证书颁发机构（CA）购买

#### Https的连接过程

- 客户端向服务器发送请求，同时发送客户端支持的一套加密规则（包括对称加密、非对称加密、摘要算法）；
- 服务器从中选出一组加密算法与HASH算法，并将自己的身份信息以证书的形式发回给浏览器。证书里面包含了网站地址，加密公钥（用于非对称加密），以及证书的颁发机构等信息（证书中的私钥只能用于服务器端进行解密）；
- 客户端验证服务器的合法性，包括：证书是否过期，CA 是否可靠，发行者证书的公钥能否正确解开服务器证书的“发行者的数字签名”，服务器证书上的域名是否和服务器的实际域名相匹配；
- 如果证书受信任，或者用户接收了不受信任的证书，浏览器会生成一个`随机密钥`（用于对称算法），并用服务器提供的公钥加密（采用非对称算法对密钥加密）；使用Hash算法对握手消息进行`摘要`计算，并对摘要使用之前产生的密钥加密（对称算法）；将加密后的随机密钥和摘要一起发送给服务器；
- 服务器使用自己的私钥解密，得到对称加密的密钥，用这个密钥解密出Hash摘要值，并验证握手消息是否一致；如果一致，服务器使用对称加密的密钥加密握手消息发给浏览器；
- 浏览器解密并验证摘要，若一致，则握手结束。之后的数据传送都使用对称加密的密钥进行加密


#### 状态码
- 3xx状态码：301 永久转移；302 临时转移
- 4xx状态码：客户端错误。400 Bad Request；401 Unauthorized；403 Forbidden；404 Not Found；
- 5xx状态码：服务端错误。500 internal error；501 Not Implemented; 502 Bad Gateway(从远程服务器收到无效响应); 503 service unavailable; 504 Gateway timeout