---
layout: post
current: post
cover: 'https://picsum.photos/id/456/2000/820'
navigation: True
title: 'Webpack Plugin 介紹 - 打包資訊工具篇'
date: 2018-10-20 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### Webpack-Bar

更為精緻的進度條 XD，使用進度條以及更精確的資訊顯示在 Terminal，明確的列出當前處理檔案為何，讓我們更容易掌握打包進度。

￼![webpackbar](https://i.imgur.com/Rejo0rG.png)

先安裝 Plugin

```javascript
npm i webpackbar -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var WebpackBar = require('webpackbar')
module.exports = {
    plugins: [
        new WebpackBar({
            color: 'green',
            profile: true
        })
    ]
}
```

可客制化一些樣式，透過 `profile` 的設定，打包完成後可以秀出相關打包資訊。

￼![webpackbar](https://i.imgur.com/hXuxXKf.png)

[詳細使用方式請看這裡](https://github.com/nuxt/webpackbar)

---

### Webpack-Dashboard

將 Webpack 打包資訊，整合成 Dashboard，作者說：使用這個套件，就像在 NASA 工作一樣 XD

##### 未使用 Webpack-Dashboard

￼![Webpack-Dashboard](https://i.imgur.com/f0EU9ND.png)

##### 使用 Webpack-Dashboard，顯示打包資訊的介面十分科技感

￼![Webpack-Dashboard](https://i.imgur.com/BK4iszv.png)

先安裝 Plugin

```javascript
npm i webpack-dashboard -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var DashboardPlugin = require('webpack-dashboard/plugin')
module.exports = {
    plugins: [
        new DashboardPlugin({
            port: 3001
        })
    ]
}
```

可以指定 Dashboard 開啟的 port。

[詳細使用方式請看這裡](https://github.com/FormidableLabs/webpack-dashboard)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
