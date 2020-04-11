---
layout: post
current: post
cover: 'https://picsum.photos/id/446/2000/820'
navigation: True
title: 'ASP.NET MVC 導入 PostCSS'
date: 2018-10-10 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### 關於 PostCSS

![PostCSS](https://i.imgur.com/YToWhpN.png)

> #### PostCSS 是一個使用 JavaScript 轉換 CSS 的工具

PostCSS 是一套後處理器，將 CSS 解析為 AST 抽象語法樹後，再透過 Plugin 去做後續處理。下面先來介紹一下預處理器及後處理器：

#### Pre-processor

較常見的預處理器 ( Pre-processor ) 是 `SASS` `SCSS` `LESS`，提供我們許多原生 CSS 無法做到的功能，比如：

-   Variable 使用變數
-   Nesting 允許巢狀寫法
-   使用 Mixin
-   定義 Function
-   Operation 可以對長度、寬度甚至色碼做一些加減乘除運算式

在大型專案中，一般 CSS 容易寫出許多重複、維護性差的程式碼，透過預處理器所提供的功能，可以讓程式碼更簡潔，並且複用性較佳、較容易維護。所以我們可以撰寫 SASS / SCSS / LESS 語法，最後透過建構工具 ( 如 Webpack ) 來 Compile 成 CSS。範例如下：

##### example.scss

```scss
$white: #fff;
$gray: #eee;
.parent {
    color: $white;
    .child {
        color: $gray;
    }
}
```

透過 Webpack 將 scss compile 成 css 檔

##### example.css

```css
.parent {
    color: #fff;
}
.parent .child {
    color: #eee;
}
```

#### Post-processor

常見的後處理器 ( Post-processor ) 就是 PostCSS，將你撰寫的 CSS 透過後製的方式來做一些處理，原本的程式碼無須做任何改寫。範例如下：

##### example.css

```css
.target {
    border-radius: 1em;
}
```

透過 PostCSS 後製添加瀏覽器前綴

##### example.css

```css
.target {
    -moz-border-radius: 1em;
    -webkit-border-radius: 1em;
    border-radius: 1em;
}
```

### 如何使用 PostCSS

##### 安裝 PostCSS 及 Plugin

```javascript
npm i postcss postcss-loader autoprefixer -D
```

在 `package.json` 添加瀏覽器列表

##### package.json

```javascript
{
  'browserslist': [
    '> 1%',				// 支援全球使用者超過 1% 的瀏覽器版本
    'last 2 versions',	// 支援每個瀏覽器最近 2 個新版本
    'not ie <= 8'		// 不支援 IE8 (包含) 以下版本
  ]
}
```

##### webpack.common.js

```javascript
{
  test: /\.(css|scss)$/,
  use: [
    { loader: MiniCssExtractPlugin.loader },
    { loader: 'css-loader' },
    {
      loader: 'postcss-loader',
      options: {
        ident: 'postcss',
        plugins: [
          require('autoprefixer')({...options}),
        ]
      }
    }
  ]
}
```

這樣配置即可透過 PostCSS Autoprefixer 來自動添加瀏覽器前綴。

### 常用 Plugin 介紹

#### [Autoprefixer](https://github.com/postcss/autoprefixer)

應該算是最常使用的 Plugin，透過分析 CSS 並使用 `Can I Use` 中瀏覽器支援度的資料，自動添加瀏覽器前綴。

#### [Cssnext](https://github.com/MoOx/postcss-cssnext)

> ##### Use tomorrow’s CSS syntax, today.

功能就像 Babel 一樣，讓你可以在專案中使用一些 CSS 的新功能，並且無須擔心瀏覽器支援度。

#### [Stylelint](https://github.com/stylelint/stylelint)

功能強大的 CSS Linter，可以有效避免 CSS 語法錯誤並可以設定自動修復。

##### 透過檢測來發現錯誤

![](https://i.imgur.com/Oba63m4.png)

### PostCSS 的優勢

-   可與各種 Pre-processor `SASS` `SCSS` `LESS` 結合使用。
-   可集成到各種建構工具 `Webpack` `Gulp` `Grunt` 等。
-   根據專案需求使用各種 Plugin。
-   幾乎無痛導入，無須調整原本的程式碼。

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
