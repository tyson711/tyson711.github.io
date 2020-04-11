---
layout: post
current: post
cover: 'https://picsum.photos/id/443/2000/820'
navigation: True
title: '頁面緩存設定'
date: 2018-10-08 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### output filename 配置參數

-   `[name]` 文件檔名
-   `[path]` 文件路徑
-   `[ext]` 文件副檔名
-   `[hash]` 是 Webpack 編譯對象 compilation 的 hash 值，Webpack 每一次建構無論內容是否異動，都會產生新的 hash 字串帶入文件名稱，因此沒有緩存的效用。
-   `[chunkhash]` 是 chunk 的 hash 值，會根據 Webpack 建構後的內容去產生 hash 字串帶入文件名稱，因此若 chunk 內容無異動， hash 就不會改變，因此可以用來達到瀏覽器緩存的效果。
-   `[contenthash]` 根據被提取 ( Extract ) 的內容所產生的 hash

### 實測緩存效果

我們使用兩個 JS 檔案、兩個 CSS 檔來做實測，`Home/Index.js` `Product/Index.js` `Sass/Index.scss` `Sass/Product.scss` 這四個檔案，我們會異動 `Home/Index.js` `Sass/Index.scss` 這兩個檔案並記錄不同 hash 的檔名變化。

##### Home/Index.js 內容如下

```javascript
import '@Resource/Sass/Index'
export default { content: 'Index' }
```

##### Sass/Index.scss 內容如下

```css
.page-index {
    background-color: #eee;
}
```

---

#### 使用 hash

##### webpack.common.js

```javascript
output: {
  filename: '[name].[hash:8].js',
}
```

CSS 的部分透過 `mini-css-extract-plugin` 提取出來存成獨立 CSS 檔。

```javascript
new MiniCssExtractPlugin({
    filename: '[name].[hash:8].css'
})
```

執行打包

![enter image description here](https://i.imgur.com/CnpkSEv.png)

四個檔案的 hash 一模一樣。

##### Home/Index.js 異動如下

```javascript
import '@Resource/Sass/Index'
export default { content: 'change' }
```

執行打包

![](https://i.imgur.com/MEJFRFr.png)

每個檔案的 hash 值都改變了，hash 果然無法達到緩存效果。

---

#### 使用 chunkhash

##### webpack.common.js

```javascript
output: {
  filename: '[name].[chunkhash:8].js',
}
```

##### webpack.common.js

```javascript
new MiniCssExtractPlugin({
    filename: '[name].[chunkhash:8].css'
})
```

執行打包

![](https://i.imgur.com/95AlyYz.png)

Home 跟 Product 的 hash 不一樣了，chunkhash 果然是依照 chunk 內容所產生的 hash。
接著再次修改 `Home/Index.js` 並執行打包。

![](https://i.imgur.com/HJKkGUl.png)

可以發現 Home 的 JS 跟 CSS hash 都改變了，接著我們異動 `Sass/Index.scss` 內容並執行打包。

![](https://i.imgur.com/Wueqjz7.png)

可以發現不管是修改 JS 還是 CSS，兩個檔案的 hash 同時都會被異動。因為 Webpack 產生 chunkhash 時，是以整個入口文件 ( 包含 Sass/Index.scss ) 的內容去產生的，因此無論是只有 JS 被異動或是只有 CSS 被異動，chunkhash 都會改變。但這跟我們預期有落差，既然我們已經將樣式提取出來另存 CSS，那麼兩個檔案就應該能夠分別緩存才對。

---

#### 使用 contenthash

##### webpack.common.js

```javascript
output: {
  filename: '[name].[contenthash:8].js',
}
```

##### webpack.common.js

```javascript
new MiniCssExtractPlugin({
    filename: '[name].[contenthash:8].css'
})
```

執行打包

![](https://i.imgur.com/1ndNTVn.png)

可以發現 CSS 與 JS 的 hash 被獨立開來了，接著再次修改 `Home/Index.js` 並執行打包。

![](https://i.imgur.com/JHA2GEY.png)

成功了！只有 JS 的 hash 被改變，接著還原 `Home/Index.js` 內容，並修改 `Sass/Index.scss` 執行打包。

![](https://i.imgur.com/rNt0afO.png)

只有 CSS 的 hash 被改變。

---

### 結論

內容有異動的檔案 hash 會被改變，而沒有異動的檔案 hash 則維持不變，這就是我們所要的緩存效果。  
須注意樣式的部分有使用 `mini-css-extract-plugin` 提取出來獨立存 CSS 檔，就必須使用 `contenthash`。  
若沒有將樣式提取出來另存，則使用 `chunkhash` 即可。

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
