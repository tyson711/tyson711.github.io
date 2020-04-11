---
layout: post
current: post
cover: 'https://picsum.photos/id/450/2000/820'
navigation: True
title: 'Webpack-Dll-Plugin 介紹'
date: 2018-10-14 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

Webpack 打包處理的項目非常多，包括 JS 打包並 Babel 轉譯、CSS 打包並提取、處理雪碧圖、透過 html-webpack-plugin 生成 HTML 等。當專案規模越來越大，開發階段的 Bundle 速度會越來越慢，影響開發的節奏。優化 Webpack 打包速度有許多方式，今天就介紹其中一種優化效果相當好的 Webpack-Dll-Plugin。

### DllPlugin

> DllPlugin 和 DllReferencePlugin 用某種方法實現了拆分 Bundle，並大大提升了建構的速度。 ( Webpack 官網 )

DllPlugin 主要的概念是將專案中有使用到，但是不常異動的部分 ( 通常指的就是第三方套件 ) 獨立出來另外打包，再透過一份 manifest.json 來對應相關的模組。由於第三方套件完全抽離出來，所以後續我們對專案本身的程式碼打包時，速度會有相當明顯的提升。實際操作過程如下：

新增 webpack.vendor.js 專屬於第三方套件打包的設定檔

##### webpack.vendor.js

```javascript
var webpack = require('webpack')
var path = require('path')
module.exports = {
    entry: {
        Vendor: ['jquery', 'react', 'react-dom']
    },
    output: {
        filename: '[name].dll.js',
        path: path.join(__dirname, 'Resource/Vendor'),
        publicPath: '/Resource/Vendor',
        library: '[name]_dll'
    },
    plugins: [
        new webpack.DllPlugin({
            context: __dirname,
            name: '[name]_dll', // 要跟 output.library 保持一致
            path: path.join(__dirname, 'Resource/Vendor' + '/[name]_manifest.json')
        })
    ]
}
```

執行打包

```javascript
webpack --config webpack.vendor.js
```

會產生 `Vendor_manifest.json` `Vendor.dll.js` 兩個檔案

##### 打包後產生的檔案

```javascript
┝╸ Resource
   ┝╸ Vendor
      ┝╸ Vendor_manifest.json
      ┕╸ Vendor.dll.js
```

這樣就完成第三方套件的獨立打包囉，接下來處理專案程式碼的打包，並加入 `DllReferencePlugin` 的設定

##### webpack.common.js

```javascript
var webpack = require('webpack')
module.exports = {
    plugins: [
        new webpack.DllReferencePlugin({
            context: __dirname,
            manifest: require('./Resource/Vendor/Vendor_manifest.json')
        })
    ]
}
```

設定好後就可以執行專案程式碼的打包，由於第三方套件都已經抽離獨立打包好了，所以打包速度會有明顯的提升，而原本專案程式碼裡引用到第三方套件的部分，透過 `Vendor_manifest.json` 可以對應到第三方套件打包後的 ID，因此 Webpack 就可以透過 `__webpack_require__` 來引用。

### 優化前打包速度

![優化前打包速度](https://i.imgur.com/jYHh1hX.png)

### 優化後打包速度

![優化後打包速度](https://i.imgur.com/AREXBim.png)

`優化時間僅供參考`

### [Autodll-Webpack-Plugin](https://github.com/asfktz/autodll-webpack-plugin)

DllPlugin 相當好用，不過由於設定稍嫌複雜，所以有人處理了 AutoDll 的 Plugin，用法如下

##### 限 webpack V4 使用

```javascript
 npm install autodll-webpack-plugin -D
```

##### 官方範例

```javascript
var path = require('path')
var AutoDllPlugin = require('autodll-webpack-plugin')

module.exports = {
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
    publicPath: '/'
  },
  plugins: [
    new AutoDllPlugin({
      filename: '[name].dll.js',
      entry: {
        vendor: [
          'jquery',
          'react',
          'react-dom'
        ]
      }
    })
  ]
}
```

如此就不需要另外設定繁雜的 Vendor 設定檔囉 [詳細使用方式請看這裡](https://github.com/asfktz/autodll-webpack-plugin)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
