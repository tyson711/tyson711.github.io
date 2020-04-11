---
layout: post
current: post
cover: 'https://picsum.photos/id/428/2000/820'
navigation: True
title: 'ASP.NET MVC 專案架構及入口頁面設定'
date: 2018-10-03 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### 專案資料夾結構

```javascript
╸ ASP.NET.WEBPACK
  ┝╸ App_Data
  ┝╸ App_Start
  ┝╸ Attributes
  ┝╸ Controllers
  ┝╸ Models
  ┝╸ node_modules
  ┝╸ Resource            // 前端資源資料夾
     ┝╸ __BundleDev__    // 開發時使用的 Bundle
     ┝╸ __VendorDev__    // 開發時使用的 Vendor
     ┝╸ Bundle           // 正式區 Bundle
     ┝╸ Containers       // 入口頁面
     ┝╸ Components
     ┝╸ Images
     ┝╸ Sass
     ┝╸ Vendor
     ┝╸ Webpack_cache
  ┕╸ Views
  Startup.cs
  ASP.NET.WEBPACK.csproj
  webpack.common.js      // Common 設定檔
  webpack.dev.js         // Dev 設定檔
  webpack.prod.js        // Prod 設定檔
```

### 入口頁面設定

以前使用 Webpack 都是處理 SPA ( 單頁應用程式 )，還沒有實際執行過多入口頁面的專案。當時參考了 NEXTJS 的架構，每個 Views 頁面都有一個根組件 ( Container )，這些 Container 會 Import 到各別資料夾的 `Index.js`，`Index.js` 也是 Webpack 入口頁面的進入點。直接看範例比較清楚 `ASP.NET MVC` 的 `Views` 資料夾大概長這樣：

#### Views 資料夾結構

```javascript
╸ Views
  ┝╸ Account
     ┝╸ Profile.cshtml
     ┝╸ Login.cshtml
     ┕╸ Register.cshtml
  ┝╸ Cart
     ┝╸ Checkout.cshtml
     ┕╸ Complete.cshtml
  ┝╸ Home
     ┕╸ Home.cshtml
  ┝╸ Member
  ┝╸ Order
  ┕╸ Product
```

`Resource/Containers` 資料夾則比照 `Views` 資料夾

#### Containers 資料夾結構

```javascript
╸ Containers
  ┝╸ Account
     ┝╸ Index.js
     ┝╸ Profile.js
     ┝╸ Login.js
     ┕╸ Register.js
  ┝╸ Cart
     ┝╸ Index.js
     ┝╸ Checkout.js
     ┕╸ Complete.js
  ┝╸ Home
     ┕╸ Index.js
  ┝╸ Member
  ┝╸ Order
  ┕╸ Product
```

每個 Container 都 export default 一個根組件

##### Account/Profile.js

```javascript
export default class Profile extends React.Component{
    ...
}
```

透過各資料夾的 `Index.js` 去 import 該資料夾的 Container

##### Account/Index.js

```javascript
import Profile from './Profile'
import Login from './Login'
import Register from './Register'
```

Webpack entry 以各資料夾名稱為 Key 值，設定 Index.js 為入口頁面進入點

##### Webpack Entry

```javascript
entry: {
  Account: 'Containers/Account/Index.js',
  Cart: 'Containers/Cart/Index.js',
  Home: 'Containers/Home/Index.js',
  ...
}
```

### 使用 Glob 自動查找入口文件

依照上面的設定每次新增 / 刪除頁面，都必須修改 Webpack config，這樣有點麻煩。為了讓 Webpack 自動去查找 `Resource/Containers` 裡的 `Index.js`，我們使用 glob 這個套件自動查找入口文件，這樣以後新增 / 刪除頁面就不需要再修改 Webpack 設定檔了。

首先安裝 glob 套件

```javascript
npm i glob --save-dev
```

##### Webpack.common.js

```javascript
var glob = require('glob')
/**
 * @class getEntry
 * @description 動態查找 Containers 入口文件並打包
 */
var getEntry = function () {
    var Files = glob.sync(path.resolve(__dirname, 'Resource/Containers/**/Index.js'))
    var Entries = {}
    Files.forEach(function (file) {
        var name = /.*\/(Containers\/.*?)\.js/.exec(file)[1].split('/')[1]
        Entries[name] = file
    })
    return Entries
}
module.exports = {
    entry: getEntry(),
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'Resource/Bundle')
    }
}
```

執行打包指令

```javascript
webpack --config webpack.dev.js
```

output Bundle 檔案如下

```javascript
╸ Bundle
  ┝╸ Account.Bundle.js
  ┝╸ Cart.Bundle.js
  ┝╸ Home.Bundle.js
  ┝╸ Member.Bundle.js
  ┝╸ Order.Bundle.js
  ┕╸ Product.Bundle.js
```

[Github DEMO](https://github.com/tyson711/Webpack-with-ASP.NET-MVC)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
