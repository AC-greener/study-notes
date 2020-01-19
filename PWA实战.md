#### PWA实战

应用外壳架构（UI外壳），没有任何动态内容的部分，比如网页的头部、底部、侧边栏

##### 理解PWA

1. PWA可以渐进式增强现有的Web应用，也就是说如果你构建一个PWA，即使在一个不支持PWA的老旧浏览器上运行，它依然可以作为一个普通的网站来运行
2. PWA会指向一个清单（manifest文件），其中包含网站相关的信息，包括图标、背景/颜色
3. PWA使用了叫做Service Worker的功能（PWA的核心）
4. PWA可以离线工作。使用Service Worker，可以选择性的缓存部分网站资源来提供离线体验
5. 使用Service Worker可以拦截并缓存任何网络请求

##### SW的特点

​	SW虽然使用JS编写，但是与JS略有不同，SW有几个特点：

1. 运行在它自己的全局脚本中
2. 无法访问DOM
3. 只能使用HTTPS（为了安全）
4. 运行在worker上下文中，与JS运行在不同的线程中
5. 是完全异步的，无法使用同步API

SW能够拦截HTTP请求，从而完全控制你的网站

![image-20200119161540987](C:\Users\i-zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200119161540987.png)

##### SW的生命周期

![image-20200119161616995](C:\Users\i-zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200119161616995.png)

也就是：下载，安装，激活。可以理解为一组交通信号灯，由红到黄再到绿

第一次用户访问网站时，并不会由激活的SW来控制页面，只有当SW安装完成并且用户刷新页面或跳转到网站的其他页面时，SW才会激活并拦截请求

##### 用SW写一个demo

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

##### SW与缓存

