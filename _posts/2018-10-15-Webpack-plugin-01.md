---
layout: post
current: post
cover: 'https://picsum.photos/id/451/2000/820'
navigation: True
title: 'Webpack Plugin 介紹'
date: 2018-10-15 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### Npm-Install-Webpack-Plugin

這個套件會自動幫你安裝專案中有引用，但是尚未 `npm install` 的套件，非常的方便。

##### 官網示意圖

![示意畫面](https://cloud.githubusercontent.com/assets/15182/12540538/6a4e8f1a-c2d0-11e5-97ee-4ddaf6892645.gif)

先安裝 Plugin

```javascript
npm i npm-install-webpack-plugin -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var NpmInstallPlugin = require('npm-install-webpack-plugin')
module.exports = {
    plugins: [
        new NpmInstallPlugin({
            dev: false // Use --save or -save-dev
        })
    ]
}
```

[詳細使用方式請看這裡](https://github.com/webpack-contrib/npm-install-webpack-plugin)

---

### Ignore-Plugin

顧名思義可以讓我們忽略掉某些模組的引用。最常使用的是用來忽略 `moment` 的 `locale` 模組。

設定檔調整如下

##### webpack.common.js

```javascript
var webpack = require('webpack')
module.exports = {
    plugins: [new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/)]
}
```

##### 移除 locale 之前

![優化前](https://i.imgur.com/CAprfWk.png)

##### 移除 locale 之後

![優化後](https://i.imgur.com/05ZzJ7M.png)

或是我們可以只保留 `zh-tw`

##### webpack.common.js

```javascript
new webpack.IgnorePlugin(/^\.\/(?!zh-tw)/, /moment[\/\\]locale$/)
```

這個套件對於最後的 Bundle 體積大小優化效果相當好喔。

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
