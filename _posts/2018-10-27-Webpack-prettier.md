---
layout: post
current: post
cover: 'https://picsum.photos/id/483/2000/820'
navigation: True
title: '導入 Prettier'
date: 2018-10-27 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### 什麼是 Prettier

![Prettier](https://i.imgur.com/cnrwEb0.png)

> #### Prettier is an opinionated code formatter with support for:
>
> -   JavaScript, including ES2017
> -   JSX
> -   Flow
> -   TypeScript
> -   CSS, Less, and SCSS
> -   JSON
> -   GraphQL
> -   Markdown, including GFM
> -   YAML

Prettier 是程式碼格式化工具，可設定為存檔時自動格式化，不用再浪費時間手動格式化程式碼。同時也能夠統一團隊程式碼風格，並且可以根據團隊規範，去調整相關設定規則。一般常見的團隊規範如下：

1. 加分號還是不加分號？
2. 使用單引號還是雙引號？
3. 縮排用 TAB 還是空格？
4. 要空幾格？

使用 Prettier 這種自動格式化工具，才能確保團隊規範的落實。下面是 Prettier 格式化的範例圖

![](https://user-gold-cdn.xitu.io/2018/6/18/16412d535a2e4cc4?imageslim)

![](https://user-gold-cdn.xitu.io/2018/6/18/16412d535a4fa3e9?imageslim)

也能支援 JSX

![](https://user-gold-cdn.xitu.io/2018/6/18/16412d535b465f52?imageslim)

CSS 樣式檔也能格式化

![](https://user-gold-cdn.xitu.io/2018/6/18/16412d535931773a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

---

### 將 Prettier 導入專案

我們可以將 Prettier 結合 GIT，提交前將程式碼格式化。也可以結合編輯器，直接在存檔時就格式化。這邊介紹的方式是使用 VSCODE 來設置 Prettier

### 如何安裝

在 VSCODE 的擴充功能面板搜尋 Prettier 就可以找到，我們安裝 `Prettier - Code formatter` 這個版本。

![Prettier](https://i.imgur.com/adqJotA.png)

在 VSCODE 設定檔新增下方的配置

##### .vscode/settings.json

```javascript
{
  'editor.formatOnSave': true,
  'prettier.singleQuote': true,
  'prettier.semi': false,
  'prettier.printWidth': 120,
  'prettier.trailingComma': 'es5',
  'prettier.tabWidth': 4
}
```

`formatOnSave` 儲存時自動格式化
`singleQuote` 使用單引號
`semi` 結束是否加分號
`printWidth` 行寬
`trailingComma` 尾隨逗號
`tabWidth` 縮排空幾格

這邊要注意 VSCODE 有分為 `使用者設定` 與 `工作區設定`，Prettier 我們設定在工作區設定，by 專案來配置，才不會影響到現有的一些舊專案。

[詳細使用方式可參考這裡](https://github.com/prettier/prettier)

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
