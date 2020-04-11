---
layout: post
current: post
cover: 'https://picsum.photos/id/445/2000/820'
navigation: True
title: 'ASP.NET MVC 導入 SASS'
date: 2018-10-09 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

專案使用 SCSS 寫法，我們之前用 Gulp 來 Compile，也使用過 ASP.NET MVC [Web Compiler](https://github.com/madskristensen/WebCompiler) 來處理，後者的缺點是必須使用 Visual Studio 來開發，我個人還是習慣使用 VS Code，所以變成改 SCSS 的話就要在 Visual Studio 裡面改... Orz。

導入 Webpack 後，就可以透過 `sass-loader` 來處理 SCSS 檔，然後再透過 `css-loader` 最後 `style-loader` 將樣式引入頁面 head。

### 安裝各種 loader

```javascript
npm i sass-loader node-sass css-loader style-loader -D
```

### 將樣式引入頁面 head

設定 `sass-loader` → `css-loader` → `style-loader` 的順序處理 scss 檔，注意 Webpack 的處理順序是 **由下而上** / **由右而左**

##### webpack.common.js

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: ['style-loader', 'css-loader', 'sass-loader']
            }
        ]
    }
}
```

### 提取樣式存成獨立 CSS 檔

如果要將樣式獨立存成 CSS 檔，需要另外透過 Plugin 處理。Webpack V4 以前是使用 `extract-text-webpack-plugin`，V4 則推薦使用 `mini-css-extract-plugin`。

```javascript
npm i mini-css-extract-plugin -D
```

##### webpack.common.js

```javascript
var MiniCssExtractPlugin = require('mini-css-extract-plugin')
module.exports = {
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader']
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: '[name].css'
        })
    ]
}
```

如此即可將樣式另存為 CSS 檔，這種作法能夠有效優化 Bundle 檔案大小，記得也要參考上一篇 [頁面緩存設定](https://ithelp.ithome.com.tw/articles/10200454) 所提到的方式來處理緩存策略。

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
