---
layout: post
current: post
cover: 'https://picsum.photos/id/448/2000/820'
navigation: True
title: '使用 Sprite-Smith-Plugin 產生 CSS Sprite'
date: 2018-10-12 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### CSS Sprite

> An image sprite is a collection of images put into a single image. ( 參考自 W3schools )

CSS Sprite 技術將網站中使用的許多小圖匯集成一張大圖，藉此節省網頁請求數量，節省頻寬。比較常見的工具是使用 [SpriteSmith](https://github.com/twolfson/spritesmith) 來合併圖片，並產生合併圖片後每個小圖的對應座標。SpriteSmith 可以跟許多建構工具結合使用，像是 `Grunt` `Gulp` 以及今天要介紹的 `Sprite-Smith-Plugin`。

### 如何使用 Sprite-Smith-Plugin

先安裝 Plugin

```javascript
npm i webpack-spritesmith -D
```

設定檔調整如下

##### webpack.common.js

```javascript
var path = require('path')
var SpritesmithPlugin = require('webpack-spritesmith')
module.exports = {
    plugins: [
        new SpritesmithPlugin({
            // 定義來源圖片資料夾，可以透過glob定義那些圖片要被合併 ex: *.png
            src: {
                cwd: path.join(__dirname, 'Resource/Images/sprite'),
                glob: '*'
            },
            // 合併完成後，圖片及scss的儲存路徑
            target: {
                image: path.join(__dirname, 'Resource/Images/sprite.png'),
                css: path.join(__dirname, 'Resource/Sass/_sprite.scss')
            },
            // 可以設定一些圖片參數
            spritesmithOptions: {
                padding: 10
            },
            // 自動產生的 scss 要使用的 sprite 圖路徑
            apiOptions: {
                cssImageRef: '../Images/sprite.png'
            }
        })
    ]
}
```

設定好之後，將原本網站上面的 ICON 放在 src 來源資料夾

##### Resource/Images/

![ICON](https://i.imgur.com/5IeOLG9.png)

執行 Webpack 打包

```javascript
npm start
```

`webpack-spritesmith` 就能幫我們把合併好的圖片生成到 target 定義的 image 路徑

##### Resource/Images/

![](https://i.imgur.com/O3oWMkv.png)

同時自動生成了 \_sprite.scss 定義每個 ICON 的樣式以及相關 Mixin

##### Sass/\_sprite.scss

```sass
...
$icon-1-name: 'icon-1';
$icon-1-x: 0px;
$icon-1-y: 0px;
$icon-1-offset-x: 0px;
$icon-1-offset-y: 0px;
$icon-1-width: 50px;
$icon-1-height: 50px;
$icon-1-total-width: 170px;
$icon-1-total-height: 110px;
$icon-1-image: '../Images/sprite.png';
$icon-1: (0px, 0px, 0px, 0px, 50px, 50px, 170px, 110px, '../Images/sprite.png', 'icon-1', );
@mixin  sprites($sprites) {
  @each  $sprite  in  $sprites {
    $sprite-name: nth($sprite, 10);
    .#{$sprite-name} {
      @include  sprite($sprite);
    }
  }
}
...
```

我們可以直接在我們的樣式檔引入生成的 \_sprite.scss，最後透過下面的方式定義 class 即可使用對應的 ICON

##### Sass/Index.scss

```sass
@import './_sprite.scss';
.icon1{ @include sprite($icon-1); }
.icon2{ @include sprite($icon-2); }
.icon3{ @include sprite($icon-3); }
.icon4{ @include sprite($icon-4); }
.icon5{ @include sprite($icon-5); }
```

### 優化開發流程

透過 `webpack-spritesmith` 來產生雪碧圖，代表每次執行 Webpack 打包都會重新處理雪碧圖，但網站的 ICON 圖片並沒有這麼常更新，因此我們不需要每次打包都產生雪碧圖，只需要在圖片有異動時才執行。或是在開發階段不需要每次都執行，上線前在執行雪碧圖的產生即可，這樣也能多少優化 Webpack 的打包速度。調整設定檔如下：

##### webpack.common.js

```javascript
var path = require('path')
var SpritesmithPlugin =  require('webpack-spritesmith')
var WebpackConfig = {
  ...
}
// 判斷如果是執行上線前的打包 `npm run build` 才執行雪碧圖的產生，
// 透過這樣的設定即可自行定義產生雪碧圖的時間點。
if( process.env.npm_lifecycle_event === 'build' ) {
  WebpackConfig.plugins.push(
    new SpritesmithPlugin({
      src: {
        cwd: path.join(__dirname, 'Resource/Images/sprite'),
        glob: '*',
      },
      target: {
        image: path.join(__dirname, 'Resource/Images/sprite.png'),
        css: path.join(__dirname, 'Resource/Sass/_sprite.scss'),
      },
      spritesmithOptions: { padding: 10 },
      apiOptions: { cssImageRef: '../Images/sprite.png' }
    })
  )
}
module.exports = WebpackConfig
```

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
