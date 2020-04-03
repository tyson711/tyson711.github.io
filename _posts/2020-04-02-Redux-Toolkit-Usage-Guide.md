---
layout: post
current: post
cover: 'assets/images/running-01.jpg'
navigation: True
title: '[譯] Redux Toolkit Usage Guide'
date: 2020-04-02 10:00:00
tags: [React Redux-Toolkit]
class: post-template
subclass: 'post tag-getting-started'
author: tyson
---

## Redux Toolkit 使用指南

---

Redux 核心庫是刻意沒有太多的封裝，讓你可以決定該如何處理所有內容，例如設定 Store、State 包含哪些內容以及如何定義 reducers。

在某些情況下這樣很好，因為它為你提供了靈活性，但有時我們不希望靈活性太大。有時我們只想以最簡單的方式開始上手，並提供一些好用的預設功能，或者你正在建構一個大型應用程式並且發現你正在手動編寫一堆相似的程式碼，而你希望減少這部分的工作量。

如同 [Redux Toolkit Quick Start](https://redux-toolkit.js.org/introduction/quick-start) 所述，Redux Toolkit 的目標是幫助簡化常用的 Redux 案例。它並非為你提供一個完整的 Redux 解決方案。

Redux Toolkit 自帶了幾個可以應用在專案上的功能，並且添加了一些 Redux 常用的第三方套件，讓你自行決定如何在專案中使用 Redux，無論是開發新專案或是維護大型的現有專案。

我們來看看使用 Redux Toolkit 改善 Redux 相關的程式碼的一些方法。

---

## 設定 Store

設定 Redux Store，通常包含下列幾個步驟：

-   import / combine rootReducer
-   設定中間件，需要中間件來處理非同步事件 / Log
-   配置 [Redux DevTools](https://github.com/zalmoxisus/redux-devtools-extension)
-   根據開發 / 正式環境而調整某些邏輯

### 設置 Store

官方文件 [Configuring Your Store](https://redux.js.org/recipes/configuring-your-store) 的範例顯示了典型設定 Store 的過程：

```javascript
import { applyMiddleware, createStore } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
import thunkMiddleware from 'redux-thunk'
import monitorReducersEnhancer from './enhancers/monitorReducers'
import loggerMiddleware from './middleware/logger'
import rootReducer from './reducers'

export default function configureStore(preloadedState) {
    const middlewares = [loggerMiddleware, thunkMiddleware]
    const middlewareEnhancer = applyMiddleware(...middlewares)
    const enhancers = [middlewareEnhancer, monitorReducersEnhancer]
    const composedEnhancers = composeWithDevTools(...enhancers)
    const store = createStore(rootReducer, preloadedState, composedEnhancers)
    if (process.env.NODE_ENV !== 'production' && module.hot) {
        module.hot.accept('./reducers', () => store.replaceReducer(rootReducer))
    }
    return store
}
```

這個範例很好理解，設定 Store 的過程十分繁瑣：

-   基本的 Redux `createStore` 函數採用位置參數：`(rootReducer, preloadedState, enhancer)`。有時很容易忘記哪個參數是哪個。
-   設置 middleware 和 enhancers 的過程可能會令人困惑，尤其在你要增加新配置時候。
-   Redux DevTools Extension 文檔最初建議使用 [用程式碼檢查全域命名空間，查看 Extension 是否可用](https://github.com/zalmoxisus/redux-devtools-extension#11-basic-store)。許多使用者複製貼上了這些程式碼，使得設定 Store 的程式碼更加難以閱讀。

```
const composeEnhancers = (typeof window !== 'undefined' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__) || compose;
```

### 透過 configureStore 簡化 Store 設定

`configureStore` 通過下列方式幫助解決這些問題：

-   使用一個物件來傳遞參數，具有 key/value，易讀且更容易維護。
-   讓你提供 middleware 和 enhancers 陣列，並自動呼叫 `applyMiddleware` 及 `compose`
-   預設啟用 Redux DevTools Extension

此外，`configureStore` 預設會啟用一些 middleware，每個 middleware 都有特定的功能：

-   [`redux-thunk`](https://github.com/reduxjs/redux-thunk) 是組件外部同步和非同步邏輯最常用中間件
-   在開發時，預設的 middleware 會檢查錯誤，例如改變狀態或使用不可序列化的值。

這意味著設定 Store 的程式碼會更短且易於閱讀，並且預設啟用一些很棒的功能。

最簡單的使用方法是將 rootReducer 函數作為參數傳遞 `reducer`：

```javascript
import { configureStore } from '@reduxjs/toolkit'
import rootReducer from './reducers'
const store = configureStore({
    reducer: rootReducer
})
export default store
```

也可以用下面的寫法，`configureStore` 會自動使用 `combineReducers`

```javascript
import usersReducer from './usersReducer'
import postsReducer from './postsReducer'
const store = configureStore({
    reducer: {
        users: usersReducer,
        posts: postsReducer
    }
})
```

請注意，這只適用於一層的 reducers，如果是巢狀 reducers，需要自行使用 `combineReducers` 來處理。
如果需要客製化設置 Store，可以定義其他選項，下方為 Redux Toolkit Hot Reload 範例:

```javascript
import { configureStore, getDefaultMiddleware } from '@reduxjs/toolkit'
import monitorReducersEnhancer from './enhancers/monitorReducers'
import loggerMiddleware from './middleware/logger'
import rootReducer from './reducers'

export default function configureAppStore(preloadedState) {
    const store = configureStore({
        reducer: rootReducer,
        middleware: [loggerMiddleware, ...getDefaultMiddleware()],
        preloadedState,
        enhancers: [monitorReducersEnhancer]
    })
    if (process.env.NODE_ENV !== 'production' && module.hot) {
        module.hot.accept('./reducers', () => store.replaceReducer(rootReducer))
    }
    return store
}
```

如果有自行設定 `middleware`，則 `configureStore` 只會啟用您所設定的 `middleware` 如果您想同時啟用預設的 `middleware` 可以使用 `getDefaultMiddleware` 並將結果設定在 `middleware` 的陣列當中。

---

## 編寫 Reducers

`Reducers` 是 Redux 最重要的概念，典型的 reducer 包含:

-   依據 Action 的 type 及 payload 來決定狀態如何異動，並回傳新的狀態。
-   透過 immutable 的機制來改變狀態，並且只更新那些異動的部分。

在 Reducers 中可以使用任何條件式，最常見的是 switch，這是處理多種可能性較直覺的方法，但許多人不喜歡使用 switch，Redux 文件提到了一個 "lookup table" [範例](https://redux.js.org/recipes/reducing-boilerplate#generating-reducers)，Action Type 定義為 key，value 為 Action 對應的處理函式，但由使用者自行決定是否實作該函式。

Reducers 的另一個痛點在於 immutable 更新狀態，手動逐層更新巢狀的數據相當繁瑣，且容易出錯。

### 使用 createReducer 簡化 Reducers

由於 "lookup table" 方法很流行，因此 Redux Toolkit 提供了 `createReducer`，與 Redux 文件提到的函式相仿。而且 `createReducer` 劇有一些特殊的魔法，讓功能變得更強大。它在內部使用 [Immer](https://github.com/mweststrate/immer)，Immer 透過草稿的機制，讓我們能直接異動狀態，且無須擔心改動到舊狀態。

通常，任何使用 `switch case` 的 Redux Reducer 都可以轉換為 `createReducer`，每一個 `case` 會成為傳遞給 `createReducer` 物件中的 key，並且都能透過 Immer 的機制來改變狀態。

這是一個如何使用 `createReducer` 的範例，我們將從 Todo List 典型的 switch Reducers 開始:

```javascript
function todosReducer(state = [], action) {
    switch (action.type) {
        case 'ADD_TODO': {
            return state.concat(action.payload)
        }
        case 'TOGGLE_TODO': {
            const { index } = action.payload
            return state.map((todo, i) => {
                if (i !== index) return todo
                return {
                    ...todo,
                    completed: !todo.completed
                }
            })
        }
        case 'REMOVE_TODO': {
            return state.filter((todo, i) => i !== action.payload.index)
        }
        default:
            return state
    }
}
```

注意，我們使用 `state.concat()`、`state.map()`、`state.filter()` 配合 ES6 的解構賦值，返回全新的 state 來更新代辦事項 。

使用 `createReducer` 可以大大簡化上面的範例:

```javascript
const todosReducer = createReducer([], {
    ADD_TODO: (state, action) => {
        // "mutate" the array by calling push()
        state.push(action.payload)
    },
    TOGGLE_TODO: (state, action) => {
        const todo = state[action.payload.index]
        // "mutate" the object by overwriting a field
        todo.completed = !todo.completed
    },
    REMOVE_TODO: (state, action) => {
        // Can still return an immutably-updated value if we want to
        return state.filter((todo, i) => i !== action.payload.index)
    }
})
```

嘗試更新太多巢狀的狀態結構時， Immer 的功能特別好用，看看下面令人痛苦的範例:

```
case  "UPDATE_VALUE":
  return  {
	 ...state,
	 first:  {
		  ...state.first,
		 second:  {
			  ...state.first.second,
			  [action.someId]:  {
				 ...state.first.second[action.someId],
				 fourth: action.someValue
			  }
		  }
	  }
  }
```

可以簡化為:

```javascript
updateValue(state, action)  {
	const  {someId, someValue}  = action.payload;
	state.first.second[someId].fourth  = someValue;
}
```

好多了!

在 JavaScript 中，可以在物件中定義 key 和函式 ( 這並不限於 Redux )，並且可以用不同的寫法 function / arrow function / ES6 shorthand / 動態屬性當作 key 等等。範例如下:

```javascript
const keyName =  "ADD_TODO4";
const reducerObject =  {
  // Explicit quotes for the key name, arrow function for the reducer
  "ADD_TODO1"  :  (state, action)  =>  {  }
  // Bare key with no quotes, function keyword
  ADD_TODO2  :  function(state, action){  }
  // Object literal function shorthand
  ADD_TODO3(state, action)  {  }
  // Computed property
  [keyName]  :  (state, action)  =>  {  }
}
```

使用 ES6 shorthand 應該是最簡短的程式碼，我們可以自行選擇使用哪一種方式。

### 使用 createReducer 的注意事項

儘管 Redux Toolkit 的 createReducer 幫助很大，但請記住:

-   只有在 `createReducer` 內部才能正常使用 Immer 來改變狀態，
-   Immer 不允許您返回新的狀態值給草稿 ( 應該是草稿不可被重新賦值的意思 )

更多詳細訊息，請參考 [createReducer API 參考](https://redux-toolkit.js.org/api/createReducer)

[原文出處](https://redux-toolkit.js.org/usage/usage-guide)
