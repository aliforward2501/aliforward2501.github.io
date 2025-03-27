---
title: HTTP缓存策略
date: 2021-10-22
---

# HTTP缓存

缓存是一种保存资源副本并在下次请求时直接使用该副本的技术，当Web发现请求的资源已经被缓存时，会拦截请求，返回该资源的拷贝，而不会去服务器重新下载

通过复用以前获取过的资源，可以显著提高网站和应用程序的性能。

# HTTP缓存策略

HTTP缓存策略是为了解决客户端和服务器端信息不对称的问题

## 两种机制

### 协商机制

客户端不知道本地缓存有没有被更新，必须跟服务器沟通之后才知道。即每次发送请求都要向服务器询问

- ETag & If-None-Match

    ETag是URL的Entity Tag,也就是URL的一个标识符

    服务器会在response header返回ETag，客户端会把这个ETag和返回值一起存下来，等下次请求时，在request header中使用`If-None-Match:具体的ETag值`把请求发送出去给服务器，服务器拿到请求头中的If-None-Match跟自己当前版本的ETag进行比较，如果一样的话，则返回304(即Not Modified),只返回header,不返回body（内容），这样子的话，客户端就会直接使用本地缓存；如果不一样的话，服务器就返回200和最新的内容。

- Last-Modified & If-Modified-Since

    Last-Modified存储的是资源的最后修改时间

    服务器在reponse header中返回Last-Modified，客户端发送请求时，在request header中设置`
If-Modified-Since:具体的Last-Modified值` ，服务器拿到这个值后，跟当前版本的修改时间进行比较，如果当前版本的修改时间比这个晚，那么返回200和新的内容；如果当前版本的修改时间和这个一样，那就返回304，只返回header，不返回body，客户端直接使用本地缓存

- 优先级

    如果ETag和Last-Modified都存在，那么优先使用ETag,因为ETag的精度较高，每次修改都会生成新的ETag，而如果在一秒内多次修改了内容，在Last-Modified是看不出差别的。

    ETag比较耗费服务器资源

### 强制机制

- Expires

    服务器的response header会带上`Expires: 具体的失效时间`，在这个时间之前，浏览器都不会向服务器发送请求，而是直接用本地缓存

- Cache-Control

    - 可设置Max-age=XXX,单位是秒，例如`Cache-Control:max-age=20000` 这表示当前资源在20000秒内都不用重新请求，而是直接用本地缓存

    - 也可设置immutable，表示不可变，表示让客户端一直都用本地缓存

    - 可设置no-cache,表示使用缓存前，会强制要求把请求提交给服务器进行验证，如果返回304,则使用本地缓存，否则返回200和新的内容。

    - 可设置no-store，表示不存储客户端请求或服务器响应的任何内容，即不使用任何缓存

    - public，表示响应可以被任何缓存区（例如代理服务器、CDN等）缓存

    - private，只能被个人用户缓存，不能被其他的缓存区缓存

- 优先级

    Cache-control的优先级比Expires高

### 优先级

先使用强制缓存，如果强制缓存生效，则直接使用本地缓存；否则使用协商缓存，跟服务器进行沟通，看要不要使用本地缓存。
