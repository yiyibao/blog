---

title: 前端跨域
date: 2018-09-05 17:17:00
comments: true

---

## 前端跨域请求


前几天在熟悉公司项目的时候，发现使用axios发送的请求都需要由反向代理做中间层，来访问后端的数据。看了反向代理的代码都是类似于下面这种的样式：

```
 '/api': {
        target: 'http://x.xxxx.com/',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        },
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

就是前端如果有个包含‘/api’的请求是，反向代理都会通过自动跳转到指定的目录下请求，比如现在前端有一个‘/api/something’的请求，反向代理在收到包含‘/api'的请求后，就会变成向’http://x.xxxx.com/something'的请求。

但是问题就出现了：这样开来后端代理不就是将‘api'这样的请求变成了‘http://x.xxxx.com‘这样吗？说多了就是一个地址的改变？这样就能请求后端数据了？

带着这样的疑问，我直接使用axios发送了一个这样的请求：

```
this.$axios.get('http://x.xxxx.com/something').then(function (res) {
        that.userInfo = res.data
        console.log(JSON.stringify(that.userInfo))
      }).catch(function (err) {
        console.log(err)
      })
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

然后：

![20180808112138846](https://img-blog.csdn.net/20180808112138846?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011cnBoeV9tZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

浏览器显示跨域了？？？？

感觉使用反向代理的作用不就是和我上面直接请求地址一样？

我才发现反向代理并不是那么简单。

在了解反向代理的时候，首先我们需要知道一些必备的知识：

## 同源策略

什么是同源策略：

所谓的同源指的就是端口、域名、协议三者都相同，才能说是处于同一个源中。只要三者中有一个不一样，那么就会出现跨域请求的问题；

**\*跨域限制是浏览器的限制***：这个点需要特别的指出，以前以为跨域限制是服务器端的限制请求，实在是大错特错。**\*其实当用户在请求一个不同源的请求你时，这个请求浏览器是可以发送出去的，但是当后端服务器响应回来数据的时候，浏览器会比对响应的header中"Access-Control-Allow-Origin"设置的允许访问的源，如果没有包含当前源，则拒绝将数据返回给当前端。***所以，其实数据是被浏览器接收了，只是没有给反馈给前端而已。

## 进行跨域请求的方法

当前端在进行跨域请求的时候，可以有以下几种方法解决：

1. 直接后端服务器设置header中的“Access-Control-Allow-Origin"允许访问该源。
2. JSONP：前端人都知道的一个方法，利用浏览器对于img、script等标签src请求的开恩。
3. document.domain，这个方法没有使用过。就不误人子弟了。。。
4. 代理：包括正向代理和反向代理。1).正向代理：举个例子，加入我有两个页面http://A.xxx.com和http://B.sss.com，现在A页面想要访问B页面的数据，现在肯定是跨域了，所以需要解决，加入说我们现在使用正向代理去解决，那么只需在中间加一个代理服务器，这个代理服务器和A页面是同一个域，A页面在请求的时候，先访问一个假如说是http://A.xxx.com/something的请求地址，中间服务器在接受了之后，在转发这个请求到http://B.sss.com/something,(跨域请求限制是浏览器的限制,现在中间服务器没有经过浏览器这一层),然后得到响应数据之后，再将数据通过代理服务器发送给A页面。这样就解决了跨域的问题。2).反向代理：我是通过porxyTable来设置的。其原理就是使用地址映射，还是以上面正向代理的例子为例，当我请求B页面的时候，先向一个同源地址发送请求，然后它就会将这个请求偷偷的转换成向B的请求，然后反馈回来的数据也是通过这个中间的代理层，反馈给A，其实原理和正向代理是一样的(我觉得原理没啥差别)。

以上就是前端跨域请求的解决方法，如果有啥新的理解，随时更新。

参考博客：<https://blog.csdn.net/wonking666/article/details/79159180>