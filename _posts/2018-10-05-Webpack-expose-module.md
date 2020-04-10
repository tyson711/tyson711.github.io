---
layout: post
current: post
cover: 'https://picsum.photos/id/440/2000/820'
navigation: True
title: 'ASP.NET MVC 專案架構及入口頁面設定'
date: 2018-10-05 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

在實際的專案當中，HTML 頁面上很有可能出現以下的程式碼。

![Code](https://i.imgur.com/NfISMD8.png)

直接在頁面上調用 `$`、`React`、`ReactDOM`，由於 `jQuery`、`React`、`React-dom` 都會打包進 Webpack，透過 Webpack 封裝過後，這些第三方套件無法直接在全域取得。所以我們使用 `expose-loader` 來將這些第三方套件暴露到全域。

首先安裝 `expose-loader`

```javascript
npm i expose-loader -D
```

Webpack 設定檔修改如下

##### webpack.common.js

```javascript
module.exports = {
    entry: {
        Vendor: [
            'expose-loader?$!expose-loader?jQuery!jquery',
            'expose-loader?React!react',
            'expose-loader?ReactDOM!react-dom'
        ]
    }
}
```

這樣即可將第三方套件暴露到全域。

再舉另外一個例子，在 ASP.NET MVC 專案有使用 Reactjs.NET 處理 SSR，因此必須在 `cshtml` 調用 React Components

```javascript
@Html.React("App", new
{
    model = Model,
}, containerId: "App", serverOnly: true)
```

那這些 Components 該如何暴露出來呢？
如果用 `expose-loader` 會比較麻煩，這裡可以直接使用 output 設定來處理

##### webpack.common.js

```javascript
module.exports = {
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'Resource/Bundle')
    library: 'Components',
  }
}
```

透過 library 的設定，可以把 Webpack 入口頁面 export 的 Component 暴露到全域底下的 `Components` 頁面上就可以在 window.Components 調用到 React component

```javascript
@Html.React("Components.App", new
{
    model = Model,
}, containerId: "App", serverOnly: true)
```

[Github DEMO](https://github.com/tyson711/Webpack-with-ASP.NET-MVC)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
