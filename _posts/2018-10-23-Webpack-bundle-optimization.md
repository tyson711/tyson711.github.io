---
layout: post
current: post
cover: 'https://picsum.photos/id/459/2000/820'
navigation: True
title: '優化 Webpack 打包檔案大小'
date: 2018-10-23 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

之前介紹了 [`Webpack-Dll-Plugin`](https://ithelp.ithome.com.tw/articles/10201329) [`HappPack`](https://ithelp.ithome.com.tw/articles/10203713) 來優化 Webpack 打包速度。今天要介紹的是優化 Webpack Bundle 體積大小，首先使用之前介紹的 [Webpack-Bundle-Analyzer](https://ithelp.ithome.com.tw/articles/10202623) 來觀察 Bundle 檔案結構。

##### 優化前分析結果

![](https://i.imgur.com/KQ9A0ve.png)

右邊是專案的業務程式碼，我馬賽克掉了 XD

從分析圖可以看出 Commons 非常大一包，幾乎占了整體的三分之二，而且有很大一部分是第三方套件。因此優化的第一步，就是把 node_modules 的第三方套件獨立出來。

透過下面的設定，獨立打包 `Vendor` 與 `Commons`

##### webpack.common.js

```javascript
module.export = {
    optimization: {
        splitChunks: {
            cacheGroups: {
                vendor: {
                    name: 'Vendor',
                    chunks: 'all',
                    test: /node_modules/
                },
                common: {
                    name: 'Commons',
                    chunks: 'initial',
                    minChunks: 2
                }
            }
        }
    }
}
```

##### 再次分析打包成果

![](https://i.imgur.com/VERR5OJ.png)

`Vendor` 拉出來獨立打包後，`Commons` 瘦身了不少，這樣對於頁面的緩存也比較有幫助。`Vendor` 獨立出來之後，發現到 `moment` 底下的 `locale` 也占了不少空間，由於 `locale` 是多國語言使用的設定，基本上大部分我們都用不到，因此我們直接把它 Ignore。

##### webpack.common.js

```javascript
module.export = {
    plugins: [new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/)]
}
```

透過 `IgnorePlugin` 移除 locale 後再次分析檔案結構

##### Ignore Moment locale

![](https://i.imgur.com/zjl9LIv.png)

可以看到移除 locale 後，moment 輕量了不少。透過視覺化的打包分析工具來檢查檔案結構相當的方便，大原則就是，第一眼看到什麼，就從它開始優化起 XD

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
