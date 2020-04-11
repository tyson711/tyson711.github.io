---
layout: post
current: post
cover: 'https://picsum.photos/id/453/2000/820'
navigation: True
title: 'Webpack Plugin 介紹 - 錯誤處理篇'
date: 2018-10-17 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### Friendly-Errors-Webpack-Plugin

透過這個 Plugin 可以讓 Webpack Bundle 時發生的錯誤，透過比較友善清晰的方式顯示出來。
使用方式如下：

先安裝 Plugin

```javascript
npm i friendly-errors-webpack-plugin -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var FriendlyErrorsWebpackPlugin = require('friendly-errors-webpack-plugin')
module.exports = {
    plugins: [new FriendlyErrorsWebpackPlugin()]
}
```

設定非常簡單，來看看官網的截圖效果

##### 打包成功

![Compiled Successfully](https://camo.githubusercontent.com/8626811b709addc6e4e953b1ed2d3414fc843522/687474703a2f2f692e696d6775722e636f6d2f4d6b554568597a2e676966)

##### 打包失敗

![Failed to Compile](https://camo.githubusercontent.com/c256a672a786f2cc15037e8a371a886ffe9505bd/687474703a2f2f692e696d6775722e636f6d2f5735397a3857462e676966)

也可以自定義一些成功訊息

##### webpack.common.js

```javascript
var FriendlyErrorsWebpackPlugin = require('friendly-errors-webpack-plugin')
module.exports = {
  plugins: [
    new FriendlyErrorsWebpackPlugin({
    compilationSuccessInfo: {
      messages: ['Application is running here http://localhost:3000'],
      notes: ['Bundle Success!']
    })
  ]
}
```

[詳細使用方式請看這裡](https://github.com/geowarin/friendly-errors-webpack-plugin)

---

### Error-Overlay-Webpack-Plugin

當頁面出錯時，這個 Plugin 能清楚的秀出頁面上的錯誤程式碼位置，有利於開發階段的除錯。

##### 實際使用畫面

![error-overlay-webpack-plugin示意圖](https://i.imgur.com/Vae8ndX.png)

先安裝 Plugin

```javascript
npm i error-overlay-webpack-plugin -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var ErrorOverlayPlugin = require('error-overlay-webpack-plugin')
module.exports = {
    plugins: [new ErrorOverlayPlugin()]
}
```

這樣就能正常使用囉，一般來說只會應用在開發階段，Production 記得要拿掉喔。

[詳細使用方式請看這裡](https://github.com/smooth-code/error-overlay-webpack-plugin)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
