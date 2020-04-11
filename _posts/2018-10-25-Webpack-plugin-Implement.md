---
layout: post
current: post
cover: 'https://picsum.photos/id/461/2000/820'
navigation: True
title: '動手實作 Webpack-Plugin'
date: 2018-10-25 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### 基本的 Plugin 架構

##### 實現 Hello-World-Plugin

```javascript
function HelloWorldPlugin(options) {
    this.options = options
}

HelloWorldPlugin.prototype.apply = function (compiler) {
    compiler.plugin('done', function () {
        console.log('Hello World!')
    })

    compiler.plugin('compilation', function (compilation) {
        console.log(compilation.assets)
        compilation.plugin('optimize', function () {
            console.log('Assets are being optimized.')
        })
    })
}
```

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
