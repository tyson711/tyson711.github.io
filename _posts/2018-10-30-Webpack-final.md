---
layout: post
current: post
cover: 'https://picsum.photos/id/467/2000/820'
navigation: True
title: '導入 Webpack 心得'
date: 2018-10-30 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

![](https://i.imgur.com/dCQWxIG.png)

### 上線後需評估的項目

-   網站效能是否提升 ( GTmetrix、Google Audits )
-   開發流程、與其他部門的協作是否受到影響

### 充分的跨部門溝通 ( 包含主管、前後端、上版人員 )

這次導入 Webpack 心得大概只有 **與相關人員充分的溝通，並且取得共識**
其他部門不理解或甚至反對，甚至連前端同仁對導入 Webpack 都有意見，因此最好先取得共識，確認這是團隊要走的方向。

### 擬定計畫，循序漸進導入

-   先局部優化元件再導入，才不會邊導入邊重構，浪費許多時間
-   處理 import, export 會花許多時間

### 必須完整考慮實際上版程序為何

需要那些部門的人配合，充分溝通

### 對於開發流程的影響

專案規模較大的話，開發時 bundle 會花一點時間 ( 數 10 秒 ~ 數分鐘都有可能 )，一樣需要充分溝通

### 採取何種緩存策略

緩存的設定要弄清楚，Webpack 光 hash 就有 3 種

### 使盡吃奶的力氣優化打包速度

-   優化篇提過的方法
-   DLL 優化項目一定要導入
-   Webpack 帶來的效益評估
-   最後還是充分的溝通

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
