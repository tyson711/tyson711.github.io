---
layout: post
current: post
cover: 'https://picsum.photos/id/442/2000/820'
navigation: True
title: 'Babel 設定'
date: 2018-10-07 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### 關於 Babel

看一下 Babel 官網上的定義，馬上就能明白 Babel 的功用。

![https://i.imgur.com/QyS97zI.png](https://i.imgur.com/QyS97zI.png)

在 ES6 之後，ECMAScript 版本每年都會更新，2018 年已經來到 ES9 了。而 Babel 可以將新版的 ES 功能轉譯為 ES5，這樣舊版的瀏覽器才能夠正常運作。

Babel 的功用很清楚，但究竟該怎麼配置一直都是一知半解。比如：

-   Presets 該怎麼設定？
-   專案要使用 Decorator Babel 該怎麼設定？
-   `babel-polyfill` 跟 `transform-runtime` 又是甚麼？

這次為了導入 Webpack 有稍微做一下功課，整理如下

### 先認識一下 ECMAScript

> ECMAScript 是一種由 Ecma 國際（前身為歐洲電腦製造商協會）通過 ECMA-262 標準化的指令碼程式設計語言。 ( Wikipedia )

Javascript 就是基於 ECMAScript 實作的語言，ECMAScript 標準分為幾個階段：

-   Stage 0 - Strawman ( 展示階段 )
-   Stage 1 - Proposal ( 徵求意見階段 )
-   Stage 2 - Draft ( 草案階段 )
-   Stage 3 - Candidate ( 候選階段 )
-   Stage 4 - Finished ( 定案階段 )
    ( 參考自 [ECMAScript 6 入门 - 阮一峰](http://es6.ruanyifeng.com/) )

理解 ECMAScript 標準，以後在專案中使用新版 ES 功能，只要根據該功能目前在 ECMAScript 標準中處在哪個階段，Babel 在調整相對應的配置即可。不清楚功能是在哪個階段可以到這邊查詢 [ECMAScript proposals](https://github.com/tc39/proposals)

### Babel 介紹

先介紹一下 Babel 各個模組

#### babel-core

Babel 的核心模組，提供 Babel 核心 API 給其它 Plugin 使用。

#### babel-cli

可以透過 command Line 執行 Babel 轉譯作業

#### babel-polyfill

Babel 處理轉譯 ES5 的流程時，只是處理語法上的轉譯，比如 const、let 轉譯為 var 或是箭頭函式轉譯為 function。若是像 Promise 這種新版的 API 功能，就須要透過 `babel-polyfill` 提供的功能才能實現。

#### babel-preset-env

透過 presets 的設定，就可以使用 Babel 幫我們配置好的一些功能。網路上會找到一些 babel-preset-es2015 / es2016 / es2017 的配置，官方已經建議廢棄了，目前官方唯一推薦的就是 `babel-preset-env`。

#### babel-preset-react

專案使用 React 需要配置這個功能。

#### babel-preset-stage-1

包含 preset-stage-2、preset-stage-3 等功能，因為我們專案有使用到 `transform-export-extensions` 這個功能，所以需要配置。

```javascript
export Product from 'Product'
```

等價於

```javascript
import Product from 'Product'
export Product
```

#### babel-loader

將 Babel 轉譯的功能，集成到 Webpack 打包流程。

### 總結

專案配置如下

#### .babelrc

```javascript
{
  presets: [
    [
      'env',
      {
        debug: false,
        useBuiltIns: true,
        modules: false
      }
    ],
    'react',
    'stage-0'
  ],
  plugins: ['transform-decorators-legacy']
}
```

#### webpack.common.js

```javascript
module: {
    rules: [
        {
            test: /\.(js|jsx)$/,
            include: path.join(__dirname, 'Resource'),
            exclude: /node_modules/,
            use: {
                loader: 'babel-loader',
                query: {
                    cacheDirectory: './Resource/Webpack_cache/'
                }
            }
        }
    ]
}
```

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
