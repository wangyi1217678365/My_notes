# 页面渲染
`地址栏输入域名, DNS解析域名指向对应的服务器IP, 浏览器页面跟服务器通过TCP三次握手建立连接, 发送请求到服务器, 服务器处理请求并返回资源, 浏览器页面解析资源渲染页面, 通过TCP四次挥手断开连接.`
- [DNS解析](#dns解析)
- [TCP连接](#tcp连接)
- [HTTP请求](#http请求)
- [浏览器解析渲染页面](./浏览器渲染页面.md)
## DNS解析
DNS解析实际上就是寻找你所需要的资源的过程。假设你输入www.baidu.com，而这个网址并不是百度的真实地址，互联网中每一台机器都有唯一标识的IP地址，这个才是关键，但是它不好记，乱七八糟一串数字谁记得住啊，所以就需要一个网址和IP地址的转换，也就是DNS解析。
### 具体解析
DNS解析其实是一个递归的过程.
![DNS解析过程](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/28/16a634c9285cb545~tplv-t2oaga2asx-watermark.awebp)
输入www.google.com网址后，首先在本地的域名服务器中查找，没找到去根域名服务器查找，没有再去com顶级域名服务器查找，，如此的类推下去，直到找到IP地址，然后把它记录在本地，供下次使用。大致过程就是. -> .com -> google.com. -> www.google.com.。 (你可能觉得我多写 .，并木有，这个.对应的就是根域名服务器，默认情况下所有的网址的最后一位都是.，既然是默认情况下，为了方便用户，通常都会省略，浏览器在请求DNS的时候会自动加上)
### DNS优化
既然已经懂得了解析的具体过程，我们可以看到上述一共经过了N个过程，每个过程有一定的消耗和时间的等待，因此我们得想办法解决一下这个问题！
1. DNS缓存
`DNS存在着多级缓存，从离浏览器的距离排序的话，有以下几种: 浏览器缓存，系统缓存，路由器缓存，IPS服务器缓存，根域名服务器缓存，顶级域名服务器缓存，主域名服务器缓存。`
    1. 在你的chrome浏览器中输入:chrome://dns/，你可以看到chrome浏览器的DNS缓存。
    2. 系统缓存主要存在/etc/hosts(Linux系统)中
1. DNS负载均衡**   
不知道你们有没有注意这样一件事，你访问baidu.com的时候，每次响应的并非是同一个服务器（IP地址不同），一般大公司都有成百上千台服务器来支撑访问，假设只有一个服务器，那它的性能和存储量要多大才能支撑这样大量的访问呢？DNS可以返回一个合适的机器的IP给用户，例如可以根据每台机器的负载量，该机器离用户地理位置的距离等等，这种过程就是DNS负载均衡
---
## TCP连接
TCP提供一种可靠的传输，这个过程涉及到三次握手，四次挥手。    
### 三次握手   
1. 第一次握手：客户端发送syn包(Seq=x)到服务器，并进入SYN_SEND状态，等待服务器确认
2. 第二次握手：服务器收到syn包，必须确认客户的SYN（ack=x+1），同时自己也发送一个SYN包（Seq=y），即SYN+ACK包，此时服务器进入SYN_RECV状态
3. 第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=y+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手
> 握手过程中传送的包里不包含数据，三次握手完毕后，客户端与服务器才正式开始传送数据。理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，TCP 连接都将被一直保持下去。
### 四次挥手
1. 第一次挥手：客户端告诉服务器我不会再发送数据过去了要断开连接了，但是客户端还可以接收数据（客户端待关闭状态）
2. 第二次挥手：服务器发送消息给客户端，通知客户端已收到请求，在这时客户端还可以接收数据（客户端待关闭状态）
3. 第三次挥手：服务器发送消息给客户端，我的数据也发送完了，不会再给你发数据了（客户端待关闭状态）
4. 第四次挥手：客户端收到服务端的消息后返回确认信息，服务端接收到确认信息后（服务器关闭状态），客户端必须经过2∗MSL（最长报文段寿命）的时间后才进入关闭状态
---
## http请求
> 首先科补一个小知识，HTTP的端口为80/8080，而HTTPS的端口为443
发送HTTP请求的过程就是构建HTTP请求报文并通过TCP协议中发送到服务器指定端口 请求报文由**请求行，请求头，请求报文组成**   

**POST和GET的区别**
- GET在浏览器回退时是无害的，而POST会再次提交请求。
- GET产生的URL地址可以被Bookmark，而POST不可以。
- GET请求会被浏览器主动cache，而POST不会，除非手动设置。
- GET请求只能进行url编码，而POST支持多种编码方式。
- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
- GET请求在URL中传送的参数是有长度限制的，而POST么有。
- 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
- GET参数通过URL传递，POST放在Request body中。
> 注意一点你也可以在GET里面藏body，POST里面带参数

**重点区别**
- 对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200(返回数据);
- 而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok(返回数据)。
### 服务器处理请求并返回HTTP报文
它会对TCP连接进行处理，对HTTP协议进行解析，并按照报文格式进一步封装成HTTP Request对象，供上层使用。这一部分工作一般是由Web服务器去进行，我使用过的Web服务器有Tomcat, Nginx和Apache等等HTTP报文也分成三份，**状态码 ，响应报头和响应报文**。

---
