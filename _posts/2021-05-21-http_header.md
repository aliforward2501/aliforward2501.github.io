---
title: HTTP Header头信息
date: 2021-05-21
---

## 请求头

- host： 请求的主机地址

- Accept: 客户端希望接收到什么类型的数据，例如:`Accept:text/xml,application/xml,application/xhtml+xml,text/html` 。对应响应头的`content-type`

- Accept-Encoding： 告知客户端可以理解的内容编码方式，通常是某种压缩算法，。浏览器申明自己接收的编码方法，通常指定压缩方法，是否指定压缩，支持什么压缩方法。例如`Accept-Encoding:gzip, deflate, br`。对应响应头的`content-Encoding`

- Accept-Language: 可接收的语言方式。例如`Accept-Language: zh-CN,zh;q=0.8`

- Accept-charset: 告知服务器可以处理的字符集

- Connection：决定当前的事务完成后，是否会关闭网络连接。如果值是keep-alive，表示为长连接方式，网络连接是持久的，不会关闭，使得同一个服务器的请求可以继续在该连接上完成

- Authorization： 身份认证，一般为token。如果服务器返回401，那必须要在请求头中传输`AUthorization`

- if-None-Match: 里面的值是ETag，服务器会与自己当前版本的ETag进行比较，从而让浏览器是否使用缓存。对应响应头的`ETag`

- if-Modified-Since：里面是资源的更新时间，服务器会与自己存储的更新时间比较，从而让浏览器是否使用缓存。对应响应头的`Last-Modified`

- User-Agent: 发送请求的浏览器信息

- referer：包含一个url，用户从该URL代表的页面出发访问当前请求的页面

- Cache-Control: 通过指定指令实现对应的缓存机制

- cookie

## 响应头

- Content-encoding：响应内容的压缩方式。例如`Content-encoding: br`

- Content-Length: 表明服务器返回消息正文的长度

- date: 创建响应报文的时间

- ETag：经过hash等算出来的资源标识，返回给浏览器进行存储

- content-type: 响应内容的类型以及字符集编码方式。例如`content-type: text/html;charset=utf-8`

- Expires: 实体内容过期的时间

- Last-Modified: 资源的最后修改时间

- Allow: 用于枚举资源所支持的http请求方法。如果服务器返回了405，那么响应头要返回`Allow`字段，说明支持的请求方法

- Age: 指明内容在缓存代理中的存储时长
