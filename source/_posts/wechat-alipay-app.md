---
title: Mini App
date: 2018-06-29 10:22:51
type: "categories"
categories:
- wechat
- alipay
- mini app
---

近来在开发支付宝小程序（大体和微信小程序差不多），踩了些坑，记录下来，方便复盘。

> 整合阿里iconfont

为了减小小程序的体积以及提高页面加载速度，对于一些小icon图片，一律使用iconfont。

但如何按照常规的iconfont的使用方式，把iconfont下载放置到项目，在小程序中还是无法使用iconfont的。因为小程序中，`@font-face的src:url()`是不接受本地或是网上地址来载入资源的，它能接受的是`base64`化后的资源;

经过一番搜索，网上大部分是这样的做法：

    * 下载阿里iconfont，放置在项目中，然后把下载下来的文件里面的iconfont.ttf通过一个网站https://transfonter.org/，把它进行base64化，然后下载下来复制里面的内容粘贴到全局的css(app.acss/app.wcss)。最后，在把原来的下载好的iconfont资源里面的iconfont.css，除了@font-face这部分，剩余的部分全部copy到全局css文件。

上述做法，步骤繁琐，手工活贼多，扩展性也不好，设计师改了一个图标，那就要重新重复上述操作。

上述流程，其实也可以看做是工程流程的一部分，所以可以使用gulp来进行自动化构建完成这些繁琐的步骤。

如下图，gulp文件的配置
![](http://p150tzuds.bkt.clouddn.com//wechat_app/alipay_gulp.png)

以及整个项目的结构
![](http://p150tzuds.bkt.clouddn.com//wechat_app/alipay_project_root.png)

以及专门放置iconfont的文件
![](http://p150tzuds.bkt.clouddn.com//wechat_app/iconfont_resource.png)

整个gulp的部分作用就是把iconfont.tff自动进行base64化，而不需要去网站上进行转化。其中，`app.css`是放置全局css的，通过gulp，会合并iconfont的文件，一起生产小程序专用的全局css文件-- app.acss.(如果要生成微信小程序的，就改gulp文件，把生成文件的名字从app.acss变为app.wcss即可)

参考：

    * [文章1](https://www.jianshu.com/p/90da43965899)

    * [文章2](https://juejin.im/entry/5a54b73b6fb9a01ca7135335)


> gulp配置@import路径的根目录

为了简洁地引入全局的less文件`index.less`, 避免使用相对路径;

根据less官网的说明，可以给less配置`rootpath`来指明@import的根目录
```javascript
    // 在gulpfile文件的less部分配置,具体内容查看上面的截图
    .pipe(less({
        globalVars:{
            // 配置全局less变量
        },
        rootpath:path.resolve(__dirname,'./')
    }))
```

通过上述配置，在我的小程序引入根目录下的styles文件夹的index.less文件就可以这样写

`@import "styles/index.less"`,而不是这样写`@import "../../styles/index.less"`

参考：

    * [文章1](https://www.v2ex.com/t/223292)
    
    * [官网](http://lesscss.org/usage/#using-less-in-the-browser-options)