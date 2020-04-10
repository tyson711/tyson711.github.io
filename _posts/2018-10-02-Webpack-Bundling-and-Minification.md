---
layout: post
current: post
cover: 'https://picsum.photos/id/437/2000/820'
navigation: True
title: 'Webpack & ASP.NET Bundling and Minification'
date: 2018-10-02 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### ASP.NET Bundling and Minification

#### 缺點

-   新增刪除模組，須手動修改 BundleConfig
-   引用開源程式模組不方便
-   人工區分 Common 共用檔，容易造成同樣模組重複打包，影響效能
-   只能處理 JS 及 CSS

#### 優點

-   開發模式不會 Bundle，開發速度較快，不過須注意相依檔案引入順序

##### BundleConfig

![BundleConfig](https://i.imgur.com/juKtwij.png)

---

### Webpack

#### 優點

-   前端主流打包工具
-   可打包 JS, JSX, CSS, 圖片
-   自動提取 Common 共用檔
-   使用 import / export 來引入相依模組
-   許多擴充的 Loader 及 Plugin 可根據專案需求客製化
-   可透過 Hash 檔名實現緩存

#### 缺點

-   Local 開發時執行打包，開發速度稍慢

![Webpack](https://i.imgur.com/dCQWxIG.png)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
