---
layout: post
current: post
cover: 'https://picsum.photos/id/487/2000/820'
navigation: True
title: 'HappyPack 介紹'
date: 2018-10-21 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### 關於 HappyPack

由於 Webpack 運行在單線程的 NodeJS 上，因此跟 JavaScript 一樣，一次只能處理一件事情。在 [Webpack-Dll-Plugin 介紹](https://ithelp.ithome.com.tw/articles/10201329) 這篇中也有提到，當專案規模越來越大，開發階段的 Bundle 速度會越來越慢，影響開發的節奏。今天要介紹的 Plugin 就是能讓 Webpack 使用多進程的方式來打包，把每個任務分解給多個子進程去處理，處理完在發送給主進程，藉此來充分發揮多核心 CPU 的效能。

> 由於 JavaScript 是單線程模型，因此要發揮多核心 CPU 的效能，必須透過多進程去模擬，無法直接使用多線程來實現。

##### HappyPack 運行機制

![HappyPack 運行機制](https://i.imgur.com/1MJhAZ3.png)

官網的 HappyPack 運行機制圖可以發現，透過 HappyLoader 解析的 Modules 就能透過多進程的方式併發執行打包作業。下面我們直接導入自己的專案測試。

先安裝 Plugin

```javascript
npm i happypack -D
```

原本處理 js / jsx 檔案的 loader 設置如下

##### webpack.common.js

```javascript
module: {
    rules: [
        {
            test: /\.(js|jsx)$/,
            exclude: /node_modules/,
            use: [
                {
                    loader: 'babel-loader',
                    query: {
                        cacheDirectory: './Resource/Webpack_cache/'
                    }
                }
            ]
        }
    ]
}
```

導入 `happypack` 之後，調整設定檔如下

##### webpack.common.js

```javascript
var HappyPack = require('happypack')
var os = require('os')
var happyThreadPool = HappyPack.ThreadPool({ size: os.cpus().length })
module.export = {
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
                exclude: /node_modules/,
                use: [
                    {
                        loader: 'happypack/loader',
                        options: {
                            id: 'happyJS'
                        }
                    }
                ]
            }
        ]
    },
    plugins: [
        new HappyPack({
            id: 'happyJS',
            loaders: [
                {
                    loader: 'babel-loader',
                    query: {
                        cacheDirectory: './Resource/Webpack_cache/'
                    }
                }
            ],
            threadPool: happyThreadPool,
            cache: true,
            verbose: true
        })
    ]
}
```

下面是可以設置的一些參數

```javascript
- id: 用來對應 loader 與 plugin 的 id
- threads: 開啟幾個子進程來處理任務 ( default: 3 )
- threadPool: 多個 HappyPack 實例，共享進程池的子進程去處理任務，防止過多資源閒置。
- tmpDir: 存放 cache 檔案的位置
- cache: `是否開啟緩存` // 建議開啟
- verbose: 是否將過程輸出在 Terminal
- loaders: 原本處理的 loader
```

[詳細使用方式請看這裡](https://github.com/amireh/happypack)，也可以參考淘寶前端團隊撰寫的 [happypack 原理解析](http://taobaofed.org/blog/2016/12/08/happypack-source-code-analysis/)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
