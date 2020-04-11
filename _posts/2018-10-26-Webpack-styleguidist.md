---
layout: post
current: post
cover: 'https://picsum.photos/id/482/2000/820'
navigation: True
title: '導入 React-Styleguidist'
date: 2018-10-26 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### React-Styleguidist

`React-Styleguidist` 提供了一個支援 HMR 的 React Component 開發環境，並且可以生成組件的 Style Guideline，讓前端工程師透過這份 Guideline 快速的瀏覽並理解專案中有哪些組件，同時理解組件使用方式。

![](https://i.imgur.com/4hRgwji.png)

React-Styleguidist 是透過 Webpack 建置，所以一定要先安裝 Webpack，然後再安裝 `react-styleguidist`

```javascript
npm i react-styleguidist -D
```

新增 React-Styleguidist 設定檔

##### styleguide.config.js

```javascript
var webpack = require('webpack')
var path = require('path')
module.exports = {
  title: 'React-Styleguidist',
  components: 'Resource/Components/**/*.js',
  require: [path.join(__dirname, 'Resource/Sass/index.scss')],
  webpackConfig: {
    ...
  }
}
```

然後透過以下指令，可以起一個具有 HMR 功能的 server 來開發 Component

```javascript
npm styleguidist server
```

最後要 Production 的指令

```javascript
npm styleguidist build
```

[詳細使用方式可參考這裡](https://github.com/styleguidist/react-styleguidist)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
