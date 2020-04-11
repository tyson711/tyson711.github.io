---
layout: post
current: post
cover: 'https://picsum.photos/id/464/2000/820'
navigation: True
title: '導入 ESLint'
date: 2018-10-28 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### ESLint 介紹

![ESLint](https://i.imgur.com/sZ0k7bL.png)

一套支援 ES6 / JSX 語法的程式碼檢測工具，具高度設定彈性與擴充性。能提前檢測出可疑的、具有潛在問題的程式碼，並顯示警告或錯誤訊息。

### Why ESLint ?

-   幫你找出語法錯誤
-   確保團隊遵循最佳實踐
-   提醒你刪掉多餘的程式碼
-   統一基本的 Coding Style
-   可以客製化團隊規範，也可以根據不同專案設置不同規範
-   ES6/JSX 支援度良好

### 將 ESLint 導入專案

我們選擇將 ESLint 與 Webpack 結合，透過 eslint-loader 在 Webpack 打包 JS/JSX 之前執行 ESLint 檢查。

首先配置 Webpack 設定檔

##### webpack.common.js

```javascript
module.export = {
  module: {
    rules: {
      {
        test: /\.(js|jsx)$/,
        loader:  'eslint-loader',
        enforce:  'pre',
        exclude: /node_modules/,
        options: {
          // fix: true,  // 是否讓 ESLint 直接 fix
        },
      }
    }
  }
}
```

在專案根目錄新增 ESLint 設定檔

##### .eslintrc.json

```javascript
{
  "env": {
    "browser": true,
    "es6": true
  },
  "extends": [
    "eslint:recommended"
  ],
  "plugins": [],
  "parserOptions": {
    "ecmaVersion": 7,
    "sourceType": "module",
    "ecmaFeatures": { "jsx": true, "modules": true }
  },
  "parser": "babel-eslint",
  "rules": {
    "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }]
  }
}
```

### 總結

![](https://i.imgur.com/dqbfdmx.png)

為了確保團隊程式碼的品質以及風格的統一，我們導入了 ESLint 以及 Prettier。

`ESLint` 確保團隊程式碼質量

`Prettier` 確保團隊程式碼風格

### 參考資料

-   [ESLint](https://eslint.org/)
-   [Airbnb JavaScript Style Guide 正體中文版](https://github.com/jigsawye/javascript)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
