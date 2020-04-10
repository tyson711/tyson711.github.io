---
layout: post
current: post
cover: 'https://picsum.photos/id/441/2000/820'
navigation: True
title: 'Webpack 設定檔配置 Common / Dev / Prod'
date: 2018-10-06 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

為了區分 develop、production 設定，我們將 Webpack 設定檔拆分為 `webpack.common.js`、`webpack.dev.js`、`webpack.prod.js` 三個檔案。
`dev` 與 `prod` 主要區別在於開發階段不須處理 Minify / Uglify、Output 的設定不同、Source-map 設定也不同，另外開發階段可以使用 Webpack-dev-server、打包分析工具等等。

我們先安裝 `webpack-merge` 來結合三個設定檔

```javascript
npm i webpack-merge -D
```

### webpack.common.js

```javascript
var CommonConfig = {
  ...common 設定
}
module.exports = CommonConfig
```

### webpack.dev.js

```javascript
var Merge = require('webpack-merge')
module.exports = Merge(CommonConfig, {
  ...develop 設定
})
```

### webpack.prod.js

```javascript
var Merge = require('webpack-merge')
module.exports = Merge(CommonConfig, {
  ...production 設定
})
```

透過 NPM Scripts 設定打包指令

### package.json

```javascript
scripts: {
  dev: 'webpack --config webpack.dev.js',
  build: 'webpack --config webpack.prod.js'
}
```

這樣配置即可透過 `npm run dev`、`npm run build` 分別啟動 `dev`、`prod` 的打包配置。

[Github DEMO](https://github.com/tyson711/Webpack-with-ASP.NET-MVC)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
