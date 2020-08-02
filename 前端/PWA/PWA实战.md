#### PWA实战

应用外壳架构（UI外壳），没有任何动态内容的部分，比如网页的头部、底部、侧边栏

如果无法衡量它，也就不能改善它。对web体验来说尤为重要

##### 1，PWA的特点

1. PWA可以渐进式增强现有的Web应用，也就是说如果你构建一个PWA，即使在一个不支持PWA的老旧浏览器上运行，它依然可以作为一个普通的网站来运行
2. PWA会指向一个清单（manifest文件），其中包含网站相关的信息，包括图标、背景/颜色
3. PWA使用了叫做Service Worker的功能（PWA的核心）
4. PWA可以离线工作。使用Service Worker，可以选择性的缓存部分网站资源来提供离线体验
5. 使用Service Worker可以拦截并缓存任何网络请求

##### 2，SW的特点

​	SW虽然使用JS编写，但是与JS略有不同，SW有几个特点：

1. 运行在它自己的全局脚本中
2. 无法访问DOM
3. 只能使用HTTPS（为了安全）
4. 运行在worker上下文中，与JS运行在不同的线程中
5. 是完全异步的，无法使用同步API

SW能够拦截HTTP请求，从而完全控制你的网站

![image-20200119161540987](C:\Users\i-zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200119161540987.png)

##### 3，SW的生命周期

![image-20200119161616995](C:\Users\i-zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200119161616995.png)

也就是：下载，安装，激活。可以理解为一组交通信号灯，由红到黄再到绿

第一次用户访问网站时，并不会由激活的SW来控制页面，只有当SW安装完成并且用户刷新页面或跳转到网站的其他页面时，SW才会激活并拦截请求

##### 4，用SW写一个demo

index.html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <script>
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('./sw.js')
        .then(registration => {
          console.log('注册成功！')
        })
        .catch(err => {
          console.log('注册失败！', err)
        })
    } 
  </script>
</body>
</html>
```

sw.js就是真正的Service Worker文件

sw.js

```javascript
self.addEventListener('fetch', e => {
  if(/\.jpg$/.test(e.request.url)) {
    e.respondWith(fetch('/img/dog.jpg'))
  }
})
```

在sw中监听了fetch时间，若请求的是jpg文件，，就拦截请求并返回一张dog的图片

##### 5，SW与缓存

常见的场景，在火车上使用手机来浏览网站，每当火车进入一个不稳定的区域时，网站需要很久才能加载出来，SW缓存能够很好的解决这个问题，确保网站对于重复访问者高效的加载

都已经由HTTP缓存了，为何还需要SW的缓存呢？一般来说HTTP的缓存时间都是由后端返回给浏览器的，是有过，而SW的缓存无需再由服务器告知浏览器资源要缓存多久，SW缓存时对HTTP缓存的增强

###### 用SW做预缓存的例子

index.html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
    
  <img src='./dog.png'/>
  <script src='./index.js'></script>
  <script>
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('./sw.js')
        .then(registration => {
          console.log('注册成功！')
        })
        .catch(err => {
          console.log('注册失败！', err)
        })
    } 
  </script>
</body>
</html>
```

sw.js

```javascript
var cacheName = 'myCache';

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(cacheName)
    .then(cache => cache.addAll([
      './index.js',
      './dog.png'
    ]))
  );
});

self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
    .then(function(response) {
      if (response) {
        return response;
      }
      return fetch(event.request);
    })
  );
});
```

当SW进入install阶段时，会把index.js和dog.png加入缓存，缓存的名字叫做myCache，如果所有的文件都成功缓存了，那么SW便会安装完成，若有任何文件下载失败，则安装过程也会失败

接下来我们监听fetch事件，使用caches.match检查传入的UTRL是否匹配当前缓存中存在的内容，如果匹配，返回缓存的资源，如果资源不存在缓存中，就发起一个fetch请求去获取资源

这个例子展示了SW在安装期间缓存重要的资源，也叫做预缓存，但前提是你要知道你缓存的资源名称，如果资源是动态的，可以**先请求资源，然后将其缓存**

###### 用SW做缓存的例子

index.html，只多了一个web字体的引用

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <link href="https://fonts.googleapis.com/css?family=Lato" rel="stylesheet">
</head>
<body>
  <img src="./dog.jpg" alt="">
  
  <script>
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('./sw.js')
        .then(registration => {
          console.log('注册成功！')
        })
        .catch(err => {
          console.log('注册失败！', err)
        })
    }
  </script>
</body>
</html>
```

sw.js

```javascript
var cacheName = 'myCache'

//缓存任何获取的新资源
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request) //当前请求是否匹配缓存中的内容?
    .then(function(response) {
      if (response) {
        return response
      }

      var fetchRequest = event.request.clone() //请求是一个流,只能使用一次,需要复制请求

      return fetch(fetchRequest).then(
        function(fetchResponse) {
          if(!fetchResponse || fetchResponse.status !== 200) { //请求失败则立刻返回错判信息
            return fetchResponse
          }

          var responseToCache = fetchResponse.clone() //复制响应

          caches.open(cacheName) //打开名为myCache的缓存
          .then(function(cache) {
            cache.put(event.request, responseToCache) //将响应添加到缓存中
          })

          return fetchResponse
        }
      )
    })
  )
})
```

这样，就可以对新的资源进行缓存，之后如果在发起对这些资源的请求，就会去SW的缓存中去拿，而不是通过网络获取资源

##### 6，对文件进行版本控制

假设你有一个main.js，将它缓存在SW中，于是每次都会中缓存中获取这个js文件，当你更新了main.js，但是SW仍然回拦截并返回老的缓存版本，将文件重命名就会解决这个问题

例如，在js的末尾加一个hash字符串

```javascript
<script src ='main-xq63bj334bty.js'
```



##### 5，manifest.json

使用（web应用清单文件）minifest.json，可以控制各种设置，如图标，启动页面和web应用的启动URL，还可以使用户将web应用安装到设备的主屏幕上

##### 6，使用SW给用户推送通知

##### 7，提升离线用户体验

##### 8，使用SW解决前端单点故障问题

**单点故障**（英语：single point of failure，缩写**SPOF**）是指系统中一旦失效，就会让整个系统无法运作的部件，换句话说，单点故障即会整体故障。

1. 什么是前端单点故障？ 

   简单理解,即因为某个静态资源(js,css,@font-face自定义字体)加载失败或者阻塞请求,导致页面主体非正常渲染.

2. 出现的症状：页面空白

   HTML文档已经加载完毕，但其他资源例如（CSS,JS,字体文件）等加载出现了阻塞，导致页面空白等待的时间。

3. 前端SPOF最频繁出现的原因是第三方内容，如果主站成功返回HTML文档，从主站返回的其他相关资源应该都成功返回，但**第三方内容往往不是由主站控制**，因此会出现不可预期的错误，所以一个网站的第三方资源不应该在主站资源之前被加载，这将有可能引起前端SPOF。

4. <img src="C:\Users\i-zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200120165720359.png" alt="image-20200120165720359" style="zoom:50%;" />

##### 9，使用SW进行数据同步

当用户离线时，如果你希望向服务器发送一些内容该怎么办呢？

比如，用户可能想在离线使用web应用时保存一些重要的信息，当他们知道再重新建立连接时，这些信息会发送给服务器。后台同步就是用来处理这种场景的

后台同步是一个新的web API，使用后台同步，可以在离线时发送信息，一旦他们重新获取网络连接，SW就会在后台将消息发送出去，可以这样理解：邮件在发件箱中排队，一旦有网络连接，就将他们发送出去

IndexDB是一个浏览器端的数据库，可以用来存储大量的结构化数据，它有如下特点：

1. IndexedDB中的操作都是异步的，避免阻塞程序
2. IndexedDB遵循同源策略 ，不可以跨域
3. IndexedDB是使用key value 也就是对象来保存数据

一个例子：



##### 10，web stream

![image-20200120185349830](C:\Users\i-zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200120185349830.png)