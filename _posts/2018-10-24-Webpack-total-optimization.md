---
layout: post
current: post
cover: 'https://picsum.photos/id/490/2000/820'
navigation: True
title: 'Webpack 優化總結'
date: 2018-10-24 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

這邊總結一下優化 Webpack 打包體積以及打包速度時，使用了哪些方法。

### 打包優化大原則

#### 縮小打包體積

盡量減少打包重複的 CODE，透過 `optimization.splitChunks` 提取 Commons 共用檔

#### 減少請求數

透過 Webpack Bundle 以及 CSS Sprite 等技術減少請求數

#### 使用緩存策略

透過 `contenthash` 結合 `Html-Webpack-Plugin` 達到緩存效果

#### 規劃開發階段打包策略

Webpack 打包過程拆分為許多任務，我們可以選擇全部執行，或是局部執行 ( 後面會在補充說明 )

### 推薦優化方式

遵照上述優化的大原則，這邊介紹一些優化打包速度/體積的一些方式

#### [Webpack-Dll-Plugin](https://ithelp.ithome.com.tw/articles/10201329)

前面文章有介紹，用來抽離第三方套件獨立打包，優化打包速度效果十分顯著。

#### [HappyPack](https://ithelp.ithome.com.tw/articles/10203713)

前面文章有介紹，允許 Webpack 採用多進程方式打包，網路上查到對於優化 Webpack watch 時的 Rebuild 有幫助。不過我實際導入時，覺得效果並不顯著 @@

#### Webpack 設定檔優化

每個 `loader` 都要設置 `include` `exclude`，限縮 Webpack 查詢檔案的範圍。設定檔的 cache 也都要開啟，尤其是 Babel 的 `cacheDirectory` 一定要開啟。

#### 如何規劃開發階段打包任務

先來看看 Webpack 打包時處理了那些任務

![](https://i.imgur.com/WqIH86w.png)

我們可以講任務拆分為：

`JS` Babel 轉譯、第三方套件打包
`CSS` SASS Compile、PostCSS、透過 Mini-css-extract-plugin 提取 CSS 檔
`HTML` 透過 Html-Webpack-Plugin 產生 Partial View
`CSS Sprite` 生成雪碧圖

每次的 `npm start` 之後 Webpack 處理了這堆任務，少則幾十秒，多則幾分鐘。平常開發是還好，若是需求一多，時間緊迫的時候，這幾分鐘會讓你急得像熱鍋上的螞蟻。因此，我們將打包指令區分如下：

`npm start` 執行全部任務打包
`npm dev:js` 只打包 JS 檔，且不處理 Babel 轉譯
`npm build` 執行全部任務打包，包含 Minify Uglify

透過細分不同的打包指令，盡可能優化打包速度。

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
