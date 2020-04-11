---
layout: post
current: post
cover: 'https://picsum.photos/id/452/2000/820'
navigation: True
title: 'Webpack Plugin 介紹 - 檔案處理篇'
date: 2018-10-16 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### Copy-Webpack-Plugin

可以將整個檔案或資料夾不須透過打包，直接複製到最後的 Bundle 資料夾。通常使用在 CSS 或是 Images 資料夾。

先安裝 Plugin

```javascript
npm i copy-webpack-plugin -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var CleanWebpackPlugin = require('copy-webpack-plugin')
module.exports = {
    plugins: [
        new CopyWebpackPlugin([
            { from: 'Resource/Images/*', to: '/Images/' },
            { from: 'Resource/Css/*', to: '/Css/' }
        ])
    ]
}
```

執行打包之後，會複製以下這兩個資料夾：

1. `Resource/Images/` 底下所有的檔案複製到 `Resource/Bundle/Images/`
2. `Resource/Css/` 底下所有的檔案則複製到 `Resource/Bundle/Css/`

[詳細使用方式請看這裡](https://github.com/webpack-contrib/copy-webpack-plugin)

---

### Clean-Webpack-Plugin

每次執行打包會先將目標資料夾清空，避免資料夾存在其他舊的 Bundle 檔案。

先安裝 Plugin

```javascript
npm i clean-webpack-plugin -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var CleanWebpackPlugin = require('clean-webpack-plugin')
module.exports = {
    plugins: [
        new CleanWebpackPlugin(['Resource/Bundle'], {
            verbose: true
        })
    ]
}
```

這樣每次打包前，都會先將 `Resource/Bundle` 資料夾整個刪除，確保最終資料夾只有最新的 Bundle 檔案。

[詳細使用方式請看這裡](https://github.com/johnagan/clean-webpack-plugin)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
