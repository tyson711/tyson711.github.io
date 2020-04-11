---
layout: post
current: post
cover: 'https://picsum.photos/id/447/2000/820'
navigation: True
title: 'ASP.NET MVC 導入 CSS Modules'
date: 2018-10-11 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

一直以來 CSS 架構都存在一些問題，比如：

-   `全域汙染`
    CSS 所有的樣式都是全域，因此可能會發生命名空間衝突，樣式互相覆蓋的問題。有時候為了解決問題，而使用了非常醜的 `!important`

-   `命名混亂`
    也由於全域汙染的原因，我們會使用一些命名風格規範，像是 `BEM`，但在多人協做開發時，控管不易，容易造成命名混亂，選擇器越來越複雜的情況。

-   `架構問題`
    一般專案大多是以 `頁面` 為單位來架構 CSS，然後把一些功能模組 ( 比如 `Form` `Mixin` `Variables` ) 等拆分出來，讓其他頁面去共用。這樣以 `頁面` 為基礎的架構，與 React、Vue 等當前熱門的前端框架以 `組件` 為基礎的架構似乎格格不入。

### 關於 CSS Modules

![CSS  Module](https://i.imgur.com/aR2Han1.png)

`CSS Modules` 跟我們專案所使用的 React 設計理念 `萬物皆組件` 更加契合。並且透過下面的機制，有效解決了上述的問題。

-   所有的樣式都是 `:local`，有效解決了全域汙染及命名混亂的問題，同時也可以透過 `:global` 來定義全域樣式。
-   className 都會透過 hash 來避免命名空間衝突，同時也可以客製化 className 生成規則。
-   樣式相依於組件，因此只要引用組件即可，無須再另外引用該組件的樣式。
-   可與其他 CSS 預處理器 ( SASS、SCSS、LESS ) 結合使用。
-   Webpack 可以透過 css-loader 來處理 CSS Modules 的機制，支援度相當好。
-   依然是撰寫 CSS 幾乎沒有學習成本。

### 如何使用 CSS Modules

##### 安裝 css-loader

```javascript
npm i css-loader -D
```

將 css-loader `modules` 設置為 true 來開啟 CSS Modules 功能，同時設置 `localIdentName` 客製化 className 產生的規則。

##### webpack.common.js

```javascript
{
  test: /\.(css|scss)$/,
  use: [
    {
      loader: 'css-loader',
      options: {
        modules: true,
        localIdentName: '[name]_[local]-[hash:base64:5]'
    }
  ]
}
```

### 實測觀察 className

我們新增一個 Component `Header` 來實測 CSS Modules 功能

##### 資料夾結構

```javascript
┝╸ Components
   ┝╸ Header
      ┝╸ Header.js
      ┕╸ Header.scss
```

在 `Header.scss` 定義兩個樣式 `.header` `.logo`

##### Header.scss

```css
.header {
    background-color: #999;
    margin: 5px 0;
}
.logo {
    display: inline-block;
    height: 50px;
    padding: 12px 30px;
    text-align: center;
    background-color: #666;
    color: white;
    font-size: 18px;
}
```

在 `Header.js` 引入 `Header.scss`，並且在 Header 裡面引用 styles 裡透過 CSS Modules 處理過的 className。

##### Header.js

```javascript
import styles from './Header.scss'
const Header = props => {
    return (
        <div className={styles.header}>
            <div className={styles.logo}>ASP.NET MVC with Webpack</div>
        </div>
    )
}
export default Header
```

import 進來的 `styles` 物件是什麼呢？其實就是原本定義的 className 對應 CSS Modules 處理過的 className

##### styles 物件

![styles Object](https://i.imgur.com/W5Wr4Rv.png)

也就是說 component 裡面去引用 styles.logo 這個樣式

##### Header.js

```javascript
<div className={styles.logo}>ASP.NET MVC with Webpack</div>
```

實際產生在頁面上的 className 是這樣

```html
<div class="Header_logo-2eVyK">ASP.NET MVC with Webpack</div>
```

透過 CSS Modules 幫我們產生獨一無二的 className，我們無須再擔心命名空間互相覆蓋的問題，實際的 UI 畫面如下

![UI](https://i.imgur.com/0XCWVjC.png)

透過 CSS Modules 處理過的 className

![DOM](https://i.imgur.com/gXilWF4.png)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
