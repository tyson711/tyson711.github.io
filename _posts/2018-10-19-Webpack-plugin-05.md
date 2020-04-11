---
layout: post
current: post
cover: 'https://picsum.photos/id/455/2000/820'
navigation: True
title: 'Webpack Plugin 介紹 - 提示工具篇'
date: 2018-10-19 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### Webpack-Notifier

一款能夠在 Bundle Successfully or Failure 時，跳出桌面級的提醒視窗，具有明顯的提示效果。底層是使用 [node-notifier](https://github.com/mikaelbr/node-notifier)，WINDOWS、MAC 系統皆可支援。

￼![Webpack-Notifier](https://i.imgur.com/VxvkZbd.png)

先安裝 Plugin

```javascript
npm i webpack-notifier -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var path = require('path')
var WebpackNotifierPlugin = require('webpack-notifier')
module.exports = {
    plugins: [
        new WebpackNotifierPlugin({
            title: 'MyProject',
            contentImage: path.join(__dirname, 'logo.png'),
            alwaysNotify: true
        })
    ]
}
```

可以自行設定提示標題、圖檔、提示時機等。

[詳細使用方式請看這裡](https://github.com/Turbo87/webpack-notifier)

---

### Progress-Bar-Webpack-Plugin

在 Webpack Bundle 時，使用進度條的方式顯示在 Terminal，讓我們更容易掌握打包完成的時間。

![Progress-Bar-Webpack-Plugin](https://camo.githubusercontent.com/cb9c82719765ad966a2771f084175c9ec935124e/687474703a2f2f692e696d6775722e636f6d2f4f495031676e6a2e676966)

先安裝 Plugin

```javascript
npm i progress-bar-webpack-plugin -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var ProgressBarPlugin = require('progress-bar-webpack-plugin')
module.exports = {
    plugins: [new ProgressBarPlugin()]
}
```

[詳細使用方式請看這裡](https://github.com/clessg/progress-bar-webpack-plugin)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
