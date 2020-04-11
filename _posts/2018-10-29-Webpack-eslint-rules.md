---
layout: post
current: post
cover: 'https://picsum.photos/id/465/2000/820'
navigation: True
title: '為專案客製化 ESLint 規則'
date: 2018-10-29 07:11:00
tags: [React Webpack Asp.Net MVC]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

![ESLint](https://i.imgur.com/sZ0k7bL.png)

延續前一篇的主題，來介紹一些我們應用在專案的 ESLint 規範

##### Possible Errors 與程式碼或邏輯錯誤有關

#### [no-console](https://cn.eslint.org/docs/rules/no-console)

禁用 console 系列語法

##### 範例

```javascript
/*eslint no-console: "error"*/

console.log('Log a debug level message.')
console.warn('Log a warn level message.')
console.error('Log an error level message.')
```

---

#### [no-debugger](https://cn.eslint.org/docs/rules/no-debugger)

禁用 debugger

##### 範例

```javascript
/*eslint no-debugger: "error"*/

function isTruthy(x) {
    debugger
    return Boolean(x)
}
```

---

#### [no-dupe-args](https://cn.eslint.org/docs/rules/no-dupe-args)

禁止 Function 中出現重名參數

##### 範例

```javascript
/*eslint no-dupe-args: "error"*/

function foo(a, b, a) {
    console.log('value of the second a:', a)
}

var bar = function (a, b, a) {
    console.log('value of the second a:', a)
}
```

---

#### [no-dupe-keys](https://cn.eslint.org/docs/rules/no-dupe-keys)

禁止物件出現重複的 KEY 值

##### 範例

```javascript
/*eslint no-dupe-keys: "error"*/

var foo = {
    bar: 'baz',
    bar: 'qux'
}

var foo = {
    bar: 'baz',
    bar: 'qux'
}

var foo = {
    0x1: 'baz',
    1: 'qux'
}
```

---

#### [no-duplicate-case](https://cn.eslint.org/docs/rules/no-duplicate-case)

禁止出現重複的 case 標籤

##### 範例

```javascript
/*eslint no-duplicate-case: "error"*/

var a = 1,
    one = 1

switch (a) {
    case 1:
        break
    case 2:
        break
    case 1: // duplicate test expression
        break
    default:
        break
}
```

---

#### [no-empty](https://cn.eslint.org/docs/rules/no-empty)

禁止出現空的區塊

##### 範例

```javascript
/*eslint no-empty: "error"*/

if (foo) {
}

while (foo) {}
```

---

#### [no-extra-semi](https://cn.eslint.org/docs/rules/no-extra-semi)

禁止非必要的分號

##### 範例

```javascript
/*eslint no-extra-semi: "error"*/

var x = 5
```

---

#### [use-isnan](https://cn.eslint.org/docs/rules/use-isnan)

必須使用 `isNaN` 檢查 NaN

##### 範例

```javascript
/*eslint use-isnan: "error"*/

if (foo == NaN) {
    // ...
}

// 正確用法
if (isNaN(foo)) {
    // ...
}
```

---

##### Best Practices 採用最佳實踐，避免程式出現一些問題

#### [no-redeclare](https://cn.eslint.org/docs/rules/no-redeclare)

禁止重複宣告變數

##### 範例

```javascript
/*eslint no-redeclare: "error"*/

var a = 3
var a = 10
```

---

##### Variables 與變數宣告有關

#### [no-undef](https://cn.eslint.org/docs/rules/no-undef)

禁止引用未宣告的變數，除非在 `/*global*/` 註解中註明

##### 範例

```javascript
/*eslint no-undef: "error"*/

var a = someFunction()
b = 10
```

[2019 IT 邦鐵人賽文章連結](https://ithelp.ithome.com.tw/articles/10199438)
