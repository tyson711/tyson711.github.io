---
layout: post
current: post
cover: 'https://picsum.photos/id/458/2000/820'
navigation: True
title: '提取 Runtime & Manifest'
date: 2018-10-22 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

### 什麼是 Runtime & Mamifest ?

看一下官網上面的說明：

> A webpack runtime and manifest that conducts the interaction of all modules.
> webpack 的 runtime 和 manifest，管理所有模块的交互。

更詳細一點的定義：

**Runtime 主要是指在瀏覽器運行時，Webpack 用來連接模組的程式碼**。Runtime 還包含模組交互時，連接模組所須的加載和解析邏輯。包括瀏覽器中已加載模組的連接，以及懶加載模組的執行邏輯。

**Manifest 則是記錄著所有模組資訊的清單**。透過 Webpack 打包過後，原始專案結構已經不存在了，Webpack 是透過 Manifest 來管理所有模組。所有的 import / require 都變成了 **webpack_require** 方法，透過這個方法以及 Manifest 中的數據來解析和加載模組。

提取 Runtime 的設定如下

##### webpack.common.js

```javascript
module.export = {
    optimization: {
        runtimeChunk: { name: 'Runtime' }
    }
}
```

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
