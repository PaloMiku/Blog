-----
title: Mix-Space：个人空间新体验
date: 2024-05-01
tags: Mix-Space
categories: 建站
abbrlink: 1
-----

　　在开始之前，我们先抛出一个问题：因为什么要使用 Mix-Space？
<!-- more -->
　　大多数外人其实只是图它好看（比如说围观到了 Innei 大佬的博客），但是当你真正上手时，会发现它与许多博客系统的不同之处，或者说，这是一套全新的个人空间系统形态。

　　大多数人其实鼓捣博客其实内心想法很简单，包括我在内也是差不多以下想法：

　　1.它要好看：就跟家里来客人，然后你家房子干不干净，漂不漂亮一样。

　　2.它要好玩：一个好玩的博客系统有效的给了博主们鼓捣和钻研的动力。

　　3.它要独特：什么 WordPress，Typecho，那都太 Low 了，我就要独特一下！

　　然后我就先看到了 [Xlog](https://xlog.app)，它确实好看，好玩，又独特。但很可惜在中国大陆地区，这玩意并不友好，先是主站域名被阻断，然后又是 IPFS 系统导致媒体在国内表现很差。然后每一次修改博文，还得 MetaMask 授权。

　　然后我们还有两位候选人：Halo 和 VanBlog。

　　Halo 的影响力其实已经不错了，但作为 Java 应用，内存占用是它的大头问题，1C1G 的小服务器是折腾不起这玩意，同样它出现的时间较短也导致了它的生态圈尚未完成。可用的主题和插件都还不够多样化。

　　VanBlog，前后端分离的博客系统，内存占用很小，强大后台面板，可以说我差一点就要把它当作我主用博客系统了，但在使用一段时间后暴露了不少问题，所以它也被我抛弃了。

　　然后，就要请出我们的主角 Mix-Space 了！它也是前后端分离的博客系统，但它的分离更加彻底，前端可以单独部署在 Serverless 平台，后端仅提供 Api 服务，性能和安全性都有不错的保障。它也好看，由 Innei 大佬操刀的 Shiro 主题，它如纸一般轻盈。Kami 主题则充满了浓浓的二次元风格。它也好玩，Xlog 同步，AI 文章摘要，前端显示博主状态，都是一个不错的个人空间选择。

　　（2024 年 7 月起个人站点已迁回到Hexo 博客系统。别问，问就是我发现我并不需要一个如此完善的「个人空间」）

### 后端

　　Mix-Space 由 Core 后端和 Theme 前端构成，后端的部署使用 Docker，部署还算简单，按照[官方文档](https://mx-space.js.org/docs/docker)部署即可。

　　值得注意的是要注意锁死MongoDB的版本号，防止在未来重部署时出现跨版本问题。

　　我使用的服务器面板是 1Panel，在网站设置这里配置好反向代理，绑定域名，证书。
	
```yaml
server {
    ## 反向代理开始
    ## WebSocket
    location /socket.io {
      proxy_pass http://127.0.0.1:2333/socket.io; 
      proxy_set_header Host $host; 
      proxy_set_header X-Real-IP $remote_addr; 
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
      proxy_set_header REMOTE-HOST $remote_addr; 
      proxy_set_header Upgrade $http_upgrade; 
      proxy_set_header Connection "upgrade"; 
      proxy_buffering off;
      proxy_http_version 1.1; 
      add_header Cache-Control no-cache; 
    }
    ## Others
    location / {
      proxy_pass http://127.0.0.1:2333; 
      proxy_set_header Host $host; 
      proxy_set_header X-Real-IP $remote_addr; 
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
      proxy_set_header REMOTE-HOST $remote_addr; 
      add_header X-Cache $upstream_cache_status; 
    }
    ## 反向代理结束
}
```


　　然后通过域名/proxy/qaqdmin 即可访问后台进行初始化操作。

　　在4:3设备上，横屏显示时，初始化步骤可能会显示异常，此时建议把设备竖过来完成初始化。

### 前端

　　Mix-Space有三款前端主题可选，但目前能够支持相关最新特性的只有Shiro（包括闭源版本Shiroi）

　　参照官方部署步骤完成Shiro部署。
	{% link https://mx-space.js.org/themes/shiro %}

　　值得注意的是，虽然Shiro可以部署到Netlify，但目前Netlify在中国大陆地区表现较差，部署到Vercel也只是「好一点」，建议搭配CDN食用主题。

### 问题

　　Mix-Space作为一个个人开发者项目，其维护积极性和稳定性都不能完全得到保障。

　　目前前端主题Shiro也分出了Shiroi这个闭源分支，一些最新特性也是Shiroi独有。