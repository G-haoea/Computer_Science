# HTTP
* [方法](#方法)     
  * [GET](#GET)     
  * [HEAD](#HEAD)     
  * [POST](#POST)     
  * [PUT](#PUT)     
  * [PATCH](#PATCH)     
  * [DELETE](#DELETE)     
  * [OPTIONS](#OPTIONS)     
  * [CONNECT](#CONNECT)     
  * [TRACE](#TRACE)     
* [GET和POST比较](#GET和POST比较)     
  * [作用](#作用)     
  * [参数](#参数)     
  * [安全](#安全)     
  * [幂等性](#幂等性)     
  * [可缓存性](#可缓存性)     
  * [XMLHttpRequest](#XMLHttpRequest)     
* [状态码](#状态码)     
  * [1XX 信息](#1XX-信息)     
  * [2XX 成功](#2XX-成功)     
  * [3XX 重定向](#3XX-重定向)     
  * [4XX 客户端错误](#4XX-客户端错误)     
  * [5XX 服务器错误](#5XX-服务器错误)     
* [应用 Cookie](#应用-Cookie)     
  * [简述](#简述)     
  * [用途](#用途)     
  * [创建过程](#创建过程)     
  * [分类](#分类)     
  * [作用域](#作用域)     
  * [JavaScript](#JavaScript)     
  * [HttpOnly](#HttpOnly)     
  * [Secure](#Secure)     
  * [Session](#Session)     
  * [浏览器禁用Cookie](#浏览器禁用Cookie)     
  * [Cookie or Session](#Cookie-or-Session)     
* [HTTPS](#HTTPS)     
  * [简述HTTP和HTTPS](#简述HTTP和HTTPS)     
  * [加密](#加密)     
  * [认证](#认证)     
  * [缺点](#缺点)     



# 方法
客户端发送的**请求报文**第一行是请求行，包含了方法字段。
## GET
`获取资源`
当前网络请求中，绝大部分使用的是GET方法。

## HEAD
`获取报文首部`
和GET方法类似，但是不返回报文实体主体部分，主要用于确认URL的有效性以及资源更新的日期时间等。

## POST
`传输实体主体`
POST主要用来传输数据，而GET主要用来获取资源。

## PUT
`上传文件`
由于自身不带验证机制，任何人都可以上传文件，因此存在安全性问题，一般不使用该方法。

## PATCH
`对资源进行部分修改`
PUT也可以用于修改资源，但是只能完全替代原始资源，PATCH允许部分修改。

## DELETE
`删除文件`
与PUT功能相反，但是同样不带验证机制。

## OPTIONS
`查询支持的方法`
查询指定的URL能够支持的方法，会返回`Allow: GET, POST, HEAD, OPTIONS`这样的内容。

## CONNECT
`要求在与代理服务器通信时建立隧道`
使用SSL和TLS协议把通信内容加密后经网络隧道传输。

## TRACE
`追踪路径`
服务器会将通信路径返回给客户端。通常不会使用TRACE，并且它容易收到XST攻击（跨站追踪）。

# GET和POST比较
## 作用
* GET用于获取资源；
* POST用于传输实体主体；

## 参数
都能使用额外的参数；
* GET的参数以**查询字符串**出现在**URL**中；
* POST参数存储在**实体主体**中；
---
因为URL只支持ASCII码；
* GET的参数中如果存在中午等字符就需要先进行编码；
* POST参数支持标准字符集；

## 安全
安全的HTTP方法不会改变服务器状态，即它只是可读的。
* GET是安全的。
* POST是不安全的：因为POST目的是传送实体主体内容，这个内容可能是用户上传的表单数据，上传成功之后，服务器可能把这个数据存储到数据库中，因此状态也就发生了改变。
---
【安全的：GET, HEAD, OPTIONS】
【不安全的：POST, PUT, DELETE】

## 幂等性
幂等的HTTP方法，同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的，即幂等方法不应该具有副作用（统计用途除外）。
* 所有的安全方法都是幂等的；
* 正确实现的情况下，GET, HEAD, PUT, DELETE等方法都是幂等的；
* POST方法不是幂等的，如果调用多次，就会增加多行记录；

## 可缓存性
如果要对响应进行缓存，需要满足以下条件：
* 请求报文的HTTP方法本身是可缓存的：GET, HEAD🉑️，PUT, DELETE❌🉑️，POST多数情况下❌🉑️；
* 响应报文的状态码是可缓存的：200，203，204，206，300，301，404，405，410，414，501；
* 响应报文的Cache-Control首部字段没有指定不进行缓存；

## XMLHttpRequest
是一个API，为客户端提供了在客户端和服务器之间传输数据的功能，提供了一个通过URL来获取数据的简单方式，并且不会使整个页面刷新。
* POST：浏览器会先发送Header再发送Data；
* GET：浏览器会把Header和Data一起发送；




# 状态码
服务器返回的**响应报文**第一行是状态行，包含了状态码以及原因短语，用来告知客户端请求的结果。
## 1XX 信息
接收的请求正在处理。
* 100 Continue：表明到目前为止都很正常，客户端可以继续发送请求或者忽略这个响应。

## 2XX 成功
请求正常处理完毕。
* 200 OK
* 204 No Content：请求已经成功处理，但是返回的响应报文不包含实体的主体部分，一般在只需要从客户端往服务器发送信息，而不需要返回数据时使用。
* 206 Partial Content：表示客户端进行了范围请求，响应报文包含由Content-Range指定范围的实体内容。

## 3XX 重定向
需要进行附加操作以完成请求。
* 301 Moved Permanently：永久性重定向。
* 302 Found：临时性重定向。
* 303 See Other：和302有相同的功能，但是303明确要求客户端应该采用GET方法获取资源。
【虽然HTTP协议规定301，302状态下重定向不允许把POST方法改成GET方法，但是大多数浏览器都会在301，302，303状态下的重定向改。】
* 304 Not Modified：如果请求报文首部包含一些条件，如果不满足条件，就返回304。
* 307 Temporary Redirect：临时重定向，和302含义类似，但是307要求浏览器不会把重定向请求的POST方法改成GET方法。

## 4XX 客户端错误
服务器无法处理请求。
* 400 Bad Request：请求报文中存在语法错误。
* 401 Unauthorised：该状态码表示发送的请求需要有认证信息，如果之前已进行过一次请求，则表示用户认证失败。
* 403 Forbidden：请求被拒绝。
* 404 Not Found

## 5XX 服务器错误
服务器处理请求错误。
* 500 Internal Server Error：服务器正在执行请求时发生错误。
* 503 Service Unavailable：服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。


# 应用 Cookie
## 简述
* HTTP协议是无状态的，主要是为了让HTTP协议尽可能简单，使得它能够处理大量食物，引入Cookie来保存**状态信息**；
* Cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器之后向同一服务器再次发起请求时被携带上，用于告知服务端两个请求是否来自同一浏览器；
* 由于之后每次请求都会需要携带Cookie数据，因此会带来额外的性能开销；
* 渐渐被淘汰，新的浏览器API已经允许开发者直接将数据存储到本地，比如使用Web storage API（本地存储和会话存储）；

## 用途
* 会话状态管理（如用户登陆状态、购物车等需要记录的信息）；
* 个性化设置（如用户自定义设置、主体等）；
* 浏览器行为跟踪（如跟踪分析用户行为等）；

## 创建过程
* 服务器发送的响应报文包含Set-Cookie首部字段，客户端得到响应报文后把Cookie内容保存到浏览器中；
* 客户端之后对同一个服务器发送请求时，会从浏览器中取出Cookie信息并通过Cookie请求首部字段发送给服务器；

## 分类
* 会话期：浏览器关闭之后它会被自动删除，即仅在会话期内有效；
* 持久性：指定过期时间（Expires）或有效期(max-age)之后就成为了持久性的Cookie；

## 作用域
* Domain标识指定了哪些主机可以接受Cookie，如果不指定，默认为当前文档的主机（不包含子域名）；
* 如果指定了Domain，则一般包含子域名；
 （例如，如果设置Domain = mozilla.org，则Cookie也包含在子域名中，如developer.mozilla.org）
 ---
 * Path标识指定了主机下的哪些路径可以接受Cookie（该URL路径必须存在于请求URL中）；
 * 以字符` / `作为路径分隔符，子路径也会被分配；
  （例如，设置Path = /docs，则下docs下的所有子地址都会被分配）

## JavaScript
浏览器通过`document.cookie`属性可以创建新的Cookie，也可以通过该属性访问非HttpOnly标记的Cookie。

## HttpOnly
* 标记为HttpOnly的Cookie不能被JavaScript脚本调用；
* 跨站脚本攻击（XSS）常常使用JavaScript的`document.cookie` API窃取用户的Cookie信息，因此使用HttpOnly标记可以在一定程度上避免XSS攻击；

## Secure
* 标记为Secure的Cookie只能通过被HTTPS协议加密过的请求发送给服务端；
* 但是即便设置了Secure标记，敏感信息也不应该通过Cookie传输，因为Cookie有其固有的不安全性，Secure标记也无法提供确实的安全保障；

## Session
* 可以将用户信息通过Cookie存储在用户浏览器中；
* 也可以利用Session存储在服务器端，存储在服务器端的信息更加安全；
* Session可以存储在服务器上的文件，数据库，内存中，也可以将Session存储在Redis这种内存型数据库中，效率会更高；
---
【过程】
* 用户登陆时，用户提交包含用户名和密码的表单，放入HTTP请求报文中；
* 服务器验证该用户名和密码，如果正确就把用户信息存在Redis中，它在Redis中的key成为Session ID；
* 服务器返回的响应报文的Set-Cookie首部字段包含了这个Session-ID，客户端收到响应报文之后将该Cookie值存入浏览器中；
* 客户端之后对同一个服务器进行请求时会包含该Cookie值，服务器收到之后提取出Session ID，从Redis中取出用户信息，继续之前的业务操作；
---
【考虑Session ID的安全性问题】
* 不能让它轻易获取；
* 经常重新生成；
* 转帐等除了Session管理用户状态，还需要对用户进行重新验证；

## 浏览器禁用Cookie
* 此时无法使用Cookie来保存信息，只能使用Session；
* 初次之外，不能再将Session ID存放到Cookie中，而是使用URL重写技术，将Session ID作为URL的参数进行传递；

## Cookie or Session
* **数据复杂性**：   
  Cookie只能存储ASCII码字符串，而Session可以存储任何类型的数据，如果考虑数据复杂性首选Session;
* **安全性**：    
  Cookie存在浏览器中，容易被恶意查看，如果非要将隐私数据存在Cookie中，可以将其加密，然后在服务器中解密；
* **开销**：    
  对于大型网站如果所有信息都存在Sesssion中，开销巨大，因此不建议所有的用户信息都存在Session中；
  
# HTTPS
## 简述HTTP和HTTPS
* HTTP有以下安全性问题：
    * 使用明文进行通信，内容可能会被**窃听**；
    * 不验证通信方身份，通信方身份可能遭遇**伪装**；
    * 无法证明报文的完整性，报文有可能遭**篡改**；
* HTTPS并不是新协议，而是让HTTP先和SSL（secure sockets layer）通信，再由SSL和TCP通信，也就是说HTTPS使用了隧道进行通信；
* 通过使用SSL，HTTPS具有了以下优点：
    * **加密**（防窃听）；
    * **认证**（防伪装）；
    * **完整性保护**（防篡改）；
![image7-1](https://github.com/iii17-grace/Computer_Science/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/image/image7-1.png)

## 加密
### 对称密钥加密
加密和解密使用同一密钥。
* 优点：运算速度快；
* 缺点：无法安全地将密钥传输给通信方；
![image7-2](https://github.com/iii17-grace/Computer_Science/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/image/image7-2.png)

### 非对称密钥加密/公开密钥加密
加密和解密使用不同的密钥。
* 公开密钥所有人都可以获得；
* 通信发送方获得接收方的公开密钥之后，就可以使用公开密钥进行加密；
* 接收方收到通信内容后使用私有密钥解密；
---
【还可以用来签名】
* 因为私有密钥无法被其他人获取，因此通信发送方使用其私有密钥进行签名；
* 通信接收方使用发送方的公开密钥对签名进行解密，就能判断这个签名是否正确；
---
* 优点：可以更安全的将公开密钥传输给通信发送方；
* 缺点：运算速度慢；
![image7-3](https://github.com/iii17-grace/Computer_Science/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/image/image7-3.png)

### HTTPS采用的加密方式是混合的加密机制
* 使用非对称密钥加密方式，传输对称密钥加密需要的Secret Key，从而保证安全性；
* 获取到Secret Key之后，使用对称密钥加密方式进行通行，保证效率；
![image7-4](https://github.com/iii17-grace/Computer_Science/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/image/image7-4.png)

## 认证
* 通过使用**证书**来对通信方进行认证；
* 数字证书认证机构CA：
  客户端与服务端双方都可信赖的第三方机构；
---
【过程】
* 服务器的运营人员向CA提出公开密钥的申请；
* CA在判明提出申请者身份之后，会对已申请的公开密钥做数字签名；
* 然后分配这个已签名的公开密钥；
* 并将该公开密钥放入公开密钥证书后绑定在一起；
* 进行HTTPS通信时，服务器会把证书发送给客户端；
* 客户端取得其中的公开密钥之后，先使用数字签名进行验证；
* 如果验证通过，就可以开始通信了；
![image7-5](https://github.com/iii17-grace/Computer_Science/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/image/image7-5.png)

## 缺点
* 需要加密解密，速度慢；
* 需要支付证书授权的高额费用；

  


  
