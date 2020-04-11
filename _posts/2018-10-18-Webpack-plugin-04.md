---
layout: post
current: post
cover: 'https://picsum.photos/id/454/2000/820'
navigation: True
title: 'Webpack Plugin 介紹 - 分析工具篇'
date: 2018-10-18 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### Webpack-Bundle-Analyzer

非常實用的視覺化打包分析工具，打包模組視覺化後，可以一目了然目前打包了那些模組、哪個模組體積過大或是重複打包，專案優化必備工具。

![webpack-bundle-analyzer](https://cloud.githubusercontent.com/assets/302213/20628702/93f72404-b338-11e6-92d4-9a365550a701.gif)

先安裝 Plugin

```javascript
npm i webpack-bundle-analyzer -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
module.exports = {
    plugins: [new BundleAnalyzerPlugin()]
}
```

這樣就能正常使用囉，只要執行打包成功後，就會將打包分析結果顯示在 8888 port ( default )。

[詳細使用方式請看這裡](https://github.com/webpack-contrib/webpack-bundle-analyzer)

---

### Speed-Measure-Plugin

透過這個套件，我們可以清楚的看到，打包整體時間、各 Plugin、Loader 處理時間分別為何，以利我們針對打包速度去做優化。

￼![示意畫面](https://i.imgur.com/PCyPCzD.png)

先安裝 Plugin

```javascript
npm i speed-measure-webpack-plugin -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var SpeedMeasurePlugin = require("speed-measure-webpack-plugin")
var smp = new SpeedMeasurePlugin()
module.exports = smp.wrap({
  ...
})
```

這樣每次打包就能夠查看每個 Plugin、Loader 耗用了多少時間

￼![示意圖](https://i.imgur.com/AREXBim.png)

[詳細使用方式請看這裡](https://github.com/stephencookdev/speed-measure-webpack-plugin)

---

### analyse

先使用 Webpack 打包指令生成 Webpack stats 分析文件

```javascript
webpack --profile --json > stats.json
```

然後到官方提供的這個網頁上傳 stats.json

[Webpack analyse](http://webpack.github.io/analyse/)

就可以透過一些視覺化資料來分析打包結果

![](https://i.imgur.com/VSlCnfB.png)
![](https://i.imgur.com/y3MiQOa.png)

[詳細使用方式請看這裡](http://webpack.github.io/analys

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
