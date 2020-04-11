---
layout: post
current: post
cover: 'https://picsum.photos/id/449/2000/820'
navigation: True
title: 'Webpack source-map 設定'
date: 2018-10-13 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

##### Webpack 官網 source-map 定義

| devtool                        | build | rebuild | production | quality                       |
| ------------------------------ | ----- | ------- | ---------- | ----------------------------- |
| none                           | +++   | +++     | yes        | bundled code                  |
| eval                           | +++   | +++     | no         | generated code                |
| cheap-eval-source-map          | +     | ++      | no         | transformed code (lines only) |
| cheap-module-eval-source-map   | o     | ++      | no         | original source (lines only)  |
| eval-source-map                | --    | +       | no         | original source               |
| cheap-source-map               | +     | o       | yes        | transformed code (lines only) |
| cheap-module-source-map        | o     | -       | yes        | original source (lines only)  |
| inline-cheap-source-map        | +     | o       | no         | transformed code (lines only) |
| inline-cheap-module-source-map | o     | -       | no         | original source (lines only)  |
| source-map                     | --    | --      | yes        | original source               |
| inline-source-map              | --    | --      | no         | original source               |
| hidden-source-map              | --    | --      | yes        | original source               |
| nosources-source-map           | --    | --      | yes        | without source content        |

###### `+++` super fast, `++` fast, `+` pretty fast, `o` medium, `-` pretty slow, `--` slow

---

### Webpack source-map 之亂

看了官網的介紹，有些 Buble Rebuild 速度比較快、有些報錯的資訊跟行數比較準確。各種的 source-map 提供了許多的選擇，但沒有實際使用過，根本不知道設定哪一個比較適合專案。所以我們針對 source-map 每一個參數實際測試，並且記錄觀察結果。

### 觀察各種 source-map 報錯狀況

Webpack 打包會處理 Babel 轉譯、minify、uglify，我在首頁新增了一個未宣告的變數如下：

```javascript
// 未使用 var、let 或 const 宣告的變數
h = 1 + 2
```

瀏覽器會看到這樣的錯誤

![ERROR](https://i.imgur.com/Xn9ZYqT.png)

根據不同的 source-map 設定，打包的速度以及顯示的錯誤資訊都不太一樣，下方就錯誤資訊的正確度做了記錄

| devtool                        | 正確顯示錯誤文件 | 正確顯示錯誤行數 | 打包速度 |
| ------------------------------ | ---------------- | ---------------- | -------- |
| none                           | X                | X                | +++      |
| `eval`                         | `O`              | X                | +++      |
| `cheap-eval-source-map`        | `O`              | X                | +        |
| cheap-module-eval-source-map   | X                | X                | O        |
| `eval-source-map`              | `O`              | `O`              | --       |
| cheap-source-map               | X                | X                | +        |
| cheap-module-source-map        | X                | X                | O        |
| inline-cheap-source-map        | X                | X                | +        |
| inline-cheap-module-source-map | X                | X                | O        |
| source-map                     | X                | X                | --       |
| inline-source-map              | X                | X                | --       |
| hidden-source-map              | X                | X                | --       |
| nosources-source-map           | X                | X                | --       |

就資訊正確度來說，只有 `eval` `cheap-eval-source-map` `eval-source-map` 這三種設定，都能夠正確地指出發生錯誤的原始文件，其他的設定最多只能看到 Bundle 後的文件名稱。而顯示錯誤行數的部分 `eval-source-map` 正確顯示原始檔的行數，其他的設定則只能顯示 Babel 編譯後的行數，而在打包速度的部分 `eval` 則是官方文件中最快速的設定。因此就這次的測試結果，開發階段我們選擇 `eval` 作為 source-map 的設定，Production 則設定為 `none` 不生成 source-map。

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
