---
title: HTTP协议+存储+跨域请求+Ajax相关介绍
date: 2023-08-11
---

## 前后端通信

指的是前后端数据交互的过程，也可以指浏览器和服务器数据交互的过程

## HTTP

### 常用的HTTP方法

- GET

    获取资源/文件

- post

- PUT更新数据

    像修改个人信息，修改密码等

- DELETE删除数据

### GET和POST的区别

- GET通过地址在请求头中携带数据，能携带的数据量和地址长度有关系，一般最多就几K。GET可以被缓存，GET请求的数据在地址栏中可以被看见，不太安全；

- POST既可以通过地址在请求头中携带数据，也可以通过请求头携带数据，能携带的数据量理论上是没有上限的。POST不会被缓存，POST比GET稍微安全点，但是也是不安全的

### HTTP状态码

- 100-199

    代表请求已被接受，需要继续处理

- 200-299

  常用的是200。表示成功

- 300-399

    重定向。常用的：301表示永久性的移动。 302表示暂时性的移动。304表示Not Modified,表示文件没有被修改，可以用

- 400-499请求错误

    常用的为404,表示文件没找到

- 500-599服务器错误

   常用的为500， 表示服务器错误

## 本地存储

### cookie

全称是HTTP Cookie，简称Cookie。是浏览器存储数据的一种方式，一般会随着浏览器每次请求发送到服务器

#### cookie的基本使用

- 写入cookie

                document.cookie="username=zs";
                document.cookie="age=18";

- 读取cookie

  可以通过`document.cookie`来获得cookie,获得的是所有的cookie，由名值对组成的字符串（以一个`;`和`空格`进行隔开）

  cookie的名和值如果包含非英文字符，则写入时需要使用`encodeURIComponent()`编码，读取的时候使用`decodeURIComponent()`解码。例如：

                document.cookie=`username=${encodeURIcomponent("张三")}";`

  cookie不支持修改、删除操作。如果要修改某个cookie,只能新建一个同名，同域，同path的cookie进行覆盖原来的cookie；如果要删除某个cookie,只需要新建一个同名、同域、同path的cookie,并且把`max-age`设置为0即可。`max-age`为-1代表仅当前浏览器使用这个cookie

#### cookie的属性

- 名称和值

- 失效(到期)cookie

    对于失效的cookie,会被浏览器清除

    如果没有设置失效时间，则称为会话cookie,存在内存中，当会话结束，也就是浏览器关闭时，cookie才会被清除。

    需要设置失效时间，可以通过`Expires`(值是date类型)或`Max-Age`(值表示距离当前多少秒)进行设置。例如：

    >document.cookie=`username=zs; expires=${new Date("2025-1-01 00:00:00"}`;

    >document.cookie="username=zs; max-age=5";

    >document.cookie=`username=zs; max-age=${24*60*60*30}`;

- domain域

    限制了不同域名访问cookie的范围。使用js只能访问当前域或父域的cookie,无法读写其他域的cookie。

    >document.cookie="username=zs; domain=www.imooc.com";

    如果你是在`www.imooc`这个域名下，是访问不到这个cookie的

- Path路径

    path限制的是同一域名下cookie的访问。只能访问到当前路径或上级路径的cookie,无法读取到下级路径的cookie。

    >document.cookie="username=zs; path=/course/list";

    如果你是在`www.imooc.com/course`这个域名以及路径下，是访问不到这个cookie的

     **只有当name和domain和path都相同的时候，才是同一个cookie**

- HttpOnly

    设置了HttpOnly属性的不能通过js访问，只能通过服务器端修改。

- Secure安全标志

    限定了只有在使用了https而不是http的情况下才可以把这个cookie发送给服务器


domain、path、secure都要满足条件，并且不能过期的cookie才能随着请求发送到服务器端

每个域名下的cookie有数量的限制，当达到限制之后再设置cookie,浏览器会自动清除以前设置的cookie。每个cookie的存储容量很小，最多只有4kB左右

### localStorage

也是一种浏览器存储数据的方式，只是存储在本地，不会随着请求一起发送到服务器

单个域名下的localStorage总大小有限制

- 设置localstorage

>localStorage.setItem("username","xiaoming");//设置localStorage的键值对

- 获得localstorage

>localStorage.getItem("username");//xiaoming,得到键对应的值。 如果是不存在的键名，会返回null

- 删除localstorage某个键值对

>localStorage.removeItem("username");//删除键值对。如果是删除不存在的key，也不会报错

- 清除localstorage中的所有键值对

>localStorage.clear();//清除所有的键值对

#### localStorage的注意事项

-  localStorage是持久化的本地存储，除非手动清除，比如通过js删除，或者清除浏览器缓存，否则数据是永远不会过期的

- 键值都是字符串类型，设置时如果传的不是字符串类型，也会自动转为字符串类型；取出来也会转为字符串类型。

- 不同域名下不能共用localStorage

- 兼容性

    IE7以及以下版本不支持localStorage,IE8开始支持

### sessionStorage

当会话结束（比如关闭浏览器）的时候，sessionStorage中的数据会被清空

也是有getItem()、setItem()、removeItem、clear()方法

## Ajax

`Asynchronous JavaScript and XML` 异步JavaScript和XML，是浏览器和服务器之间的一种异步通信方式。可以在不重新加载整个页面的情况下，对页面的某部分进行更新

XML是可扩展标记语言，是前后端数据传输时的一种数据格式。现在已经不怎么用了，比较常用的是JSON

### 使用方法

1. 先调用XMLHttpRequest()构造函数创建XHR对象

      >const xhr=new XMLHttpRequest();

2. 调用open方法

      >xhr.open(HTTP的方法,请求的URL,true或false);//true表示用异步方式，一般这里都用true

3. 发送请求,用send方法

       send的参数是通过请求体携带的数据，像GET方法只能通过请求头发送数据，数据send中可以不加参数，也可以使用null

4. 监听事件，处理响应

       当获取到响应时，会触发xhr对象的readystatechange事件，可以在当事件中对响应进行处理

       `readystatechange`事件监听的是readyState状态的变化，一共有0~4共5个状态:

        - 0表示未初始化，还没有调用open方法；
        - 1表示启动，已经调用open()，但是还没有调用send;
        - 2表示发送，已经调用了send方法，但是还没接收到响应；
        - 3表示接收，已经接收到部分响应数据；
        - 4表示完成，已经接收到全部响应数据，已经可以在浏览器中使用了

     当获取到响应的时候，响应的内容会自动填充到xhr对象的属性上

             xhr.onreadystatechange=()=>{
                if(xhr.readyState!==4) return;
                if(((xhr.status>=200)&(xhr.status小于300))||(xhr.status===304)){//可以正常使用响应数据
                  console.log(xhr.responseText);//得到的是字符串
                }
              }

    为了兼容性问题，第4点的监听事件可以放到第1点之后，也就是创建完xhr对象之后就开始监听

### GET请求

如果携带的数据是非英文字符的话，要进行编码之后再传给后端，不然会造成乱码问题，可以使用encodeURIComponent()进行编码

### POST请求

- 携带数据

    1. 通过请求体（主要）

       通过send方法

       不能直接传递对象，要转成字符串形式才可以

    2. 通过请求头

- 字符编码

    传递的有非英文字符的时候，要进行编码，也是用encodeURIComponent()

## JSON

JavaScript Object Nonation js对象表示法

### JSON的3种形式

不允许有注释，不能使用单引号，不能有undefined

- 简单值形式

- 对象形式

- 数组形式

### JSON的常用方法

- JSON.parse()

    将JSON类型转为JS对应的类型

- JSON.stringify()

    将JS数据类型转为JSON字符串类型

## 跨域

协议、域名、端口任何一个不一样，都是不同域

阻止跨域请求，本身是浏览器的一种安全策略，也就是同源策略（CORS)

### 跨域解决方案

- CORS跨域资源共享（常用）

   响应头返回的`Access-Control-Allow-Origin:*`表示允许所有的域名来跨域请求它，是通配符，没有限制。也可以设置特定的域名允许请求

   使用CORS跨域的过程：

     1. 浏览器发送跨域请求
     2. 后端在响应头中添加Access-Control-Origin头信息
     3. 浏览器接收到响应
     4. 如果是同域下的请求，浏览器不会做什么，通信就完成了；如果是跨域，浏览器会从响应头中查找是否允许跨域请求，如果允许，则通信完成，如果不允许或者没找到包含的域名，就丢弃响应结果。

   IE10以及以上版本的浏览器可以正常使用CORS

- JSONP script

   script标签跨域不会被浏览器阻止，JSONP主要就是利用script标签，加载跨域文件

   使用过程:

      1. 服务器端准备好JSONP接口
      2. 前端手动加载JSONP接口（用script接口src属性=JSONP接口），或者动态加载JSONP接口（用document.createElement创建script标签）
      3. 声明函数（函数名称可以在JSONP接口看到，函数的参数即为返回的数据

## XHR的属性

- responseType

   默认是文本类型(text或者""),指定响应内容的类型。当响应类型是text或者""时，响应内容可以用responseText接收，也可以用response接收，内容是一样的。

   可以设置为json类型，此时不能通过responseText获取响应内容。

- responseText

    文本形式的响应内容

- response

- timeout

    设置请求的超时时间（单位为毫秒）

    可以在open之后，send之前利用xhr.timeout进行设置

- withCredentials

    指定使用Ajax发送请求时是否携带cookie

    使用Ajax发送请求，默认情况下，同域时，会携带Cookie；跨域时，则不会，如果使用了`xhr.withCredentials=true;`最终是否能成功携带cookie,还要看服务器同不同意。

    如果`Access-Control-Allow-Origin`返回的是`*`,那么也是不能跨域携带cookie的，必须要让后端指定特定的域名

### XHR的方法

- abort()

    终止当前的请求，在send之后调用

- sendRequestHeader(头部字段的名称，头部字段的值)

    设置请求头信息

    在send之前使用

### XHR的事件

- load事件

   响应数据可用时触发，响应数据可用其实就是代表了readyState的状态是4了，监听load事件里面再加上状态码的判断就行了，不用再判断readyState的状态。不考虑兼容性问题的话，可以代替`readyStateChange`事件的监听

- error事件

   请求（不是指状态码）发生错误时触发

- abort事件

   调用abort()之后触发

- timeout超时事件

## FromData

`data=new FormData(获取到的form元素);//直接把这个当成data传到send的参数就行，并且不需要再额外调用setRequestHeader设置请求头。`这时的Content-Type为multipart/form-data。后面也可以通过data.append(键,值)的形式给传输的数据添加具体的键值

IE10以及以上可以支持fromdata

## axios

axios是一个基于Pormise的HTTP库，可以用在浏览器和node.js中，是一个第三方的ajax库

## fetch

也是前后端通信的一种方式，是Ajax的另外一种替代方案，基于Promise

Ajax的兼容性比fetch好，fetch没有提供abort和timeout等方法

fetch调用之后返回promise对象

看下返回的ok属性，如果为true,表示可以读取数据，不用再去判断HTTP状态码了
