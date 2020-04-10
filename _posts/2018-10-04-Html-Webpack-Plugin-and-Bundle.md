---
layout: post
current: post
cover: 'https://picsum.photos/id/439/2000/820'
navigation: True
title: 'ASP.NET MVC 專案架構及入口頁面設定'
date: 2018-10-04 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

為了把打包完的檔案引入頁面，我們透過 `Html-Webpack-Plugin` 來產生 Partial View 並引入打包檔案。`Views` 資料夾底下新增 `BundleTemp` 資料夾，使用 `BundleTemp/_JsTemplate.cshtml` 為模板來產生引入 JS 檔案的頁面。同樣的，以 `BundleTemp/_CssTemplate.cshtml` 為模板來產生引入 CSS 檔案的頁面，最後透過 `Html-Webpack-Plugin` 將 Partial View 產生到 `Views/Bundle` 資料夾。

### Views 資料夾結構

```javascript
  ┝╸ Views
     ┝╸ BundleTemp
        ┝╸ _JsTemplate.cshtml
        ┝╸ _CssTemplate.cshtml
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

安裝 `Html-Webpack-Plugin` 套件

```
npm i Html-Webpack-Plugin --save-dev
```

一般來說，Webpack 會設定如下：

```javascript
var HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    plugins: [
        new HtmlWebpackPlugin({
            filename: 'Views/Bundle/_Home.bundle.html',
            template: 'Views/BundleTemp/_JsTemplate.cshtml',
            chunks: ['Home'],
            inject: false
        })
    ]
}
```

`BundleTemp/_JsTemplate.cshtml` 裡則使用 `ejs` 的語法來接 chunks 參數

##### BundleTemp/\_JsTemplate.cshtml

```javascript
<% for (var chunk in htmlWebpackPlugin.files.chunks) { %>
  <script src="<%=htmlWebpackPlugin.files.chunks[chunk].entry %>"></script>
<%}%>
```

##### BundleTemp/\_CssTemplate.cshtml

```javascript
<% for (var chunk in htmlWebpackPlugin.files.chunks) { %>
  <link rel="stylesheet" href="<%=htmlWebpackPlugin.files.chunks[chunk].css %>"></link>
<%}%>
```

透過 `Html-Webpack-Plugin` 產生的 Partial View `Views/Bundle/Home_Js.bundle.cshtml`，就會引入 `Home.bundle.js`，而 `Views/Bundle/Home_Css.bundle.cshtml`，就會引入 `Home.css`

##### Views/Bundle/Home_Js.bundle.cshtml

```javascript
<script type="text/javascript" src="/Resource/Bundle/Home.bundle.js>"></script>
```

##### Views/Bundle/Home_Css.bundle.cshtml

```javascript
<link rel="stylesheet" href="/Resource/Bundle/Css/Home.css>"></link>
```

由於專案是多入口頁面，所以我們同樣使用 `glob` 這個方法來查找 `Views` 底下的資料夾，並結合 `Html-Webpack-Plugin` 動態生成 Partial View。

### 使用 glob 自動查找 Views 資料夾

```javascript
/**
 * @class setPartialView
 * @description 動態生成 Partial View，並引入該頁相關 Bundle 檔案
 */
setPartialView = function () {
    var TargetPage = glob.sync('Views/*/')
    var JsTemplate = 'Views/BundleTemp/_JsTemplate.cshtml'
    var CssTemplate = 'Views/BundleTemp/_CssTemplate.cshtml'
    var getPath = FileName => `./../../Views/Bundle/${FileName}.bundle.cshtml`
    TargetPage.forEach(filePath => {
        var BundleName = filePath.split('/')[1]
        if (BundleName === 'BundleTemp' || BundleName === 'Bundle') return false
        var JsFileName = getPath(BundleName + '_Js')
        var CssFileName = getPath(BundleName + '_Css')
        CommonConfig.plugins.push(
            new HtmlWebpackPlugin({
                template: JsTemplate,
                filename: JsFileName,
                chunks: [BundleName],
                inject: false
            })
        )
        CommonConfig.plugins.push(
            new HtmlWebpackPlugin({
                template: CssTemplate,
                filename: CssFileName,
                chunks: [BundleName],
                inject: false
            })
        )
    })
}
var CommonConfig = {
    entry: getEntry(),
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'Resource/Bundle')
    }
}
setPartialView() // 生成 Partial View
module.exports = CommonConfig
```

### 最後生成的 Partial View

```javascript
  ┝╸ Views
     ┝╸ Bundle
        ┝╸ Account_Js.bundle.cshtml
        ┝╸ Account_Css.bundle.cshtml
        ┝╸ Cart_Js.bundle.cshtml
        ┝╸ Cart_Css.bundle.cshtml
        ┝╸ Home_Js.bundle.cshtml
        ┝╸ Home_Css.bundle.cshtml
        ┝╸ Member_Js.bundle.cshtml
        ┝╸ Member_Css.bundle.cshtml
        ┝╸ Order_Js.bundle.cshtml
        ┝╸ Order_Css.bundle.cshtml
        ┝╸ Product_Js.bundle.cshtml
        ┝╸ Product_Css.bundle.cshtml
     ┝╸ Account
     ┝╸ Cart
     ┝╸ Home
     ┝╸ Member
     ┝╸ Order
     ┕╸ Product
```

### 最後在 cshtml 使用 @Html.Partial

```javascript
// section styles 引入在 <head>
@section styles {
  @Html.Partial("~/Views/Bundle/_Home_Css.bundle.cshtml")
}

// section scripts 引入在 </body> 上方
@section scripts {
  @Html.Partial("~/Views/Bundle/_Home_Js.bundle.cshtml")
}
```

[Github DEMO](https://github.com/tyson711/Webpack-with-ASP.NET-MVC)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
