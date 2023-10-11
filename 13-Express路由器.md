# Express 路由器

Express 開發中，將全部路由相關邏輯擠在同一個文件 (如 app.js) 是不好的，造成維護困難等問題。

Express 提供了路由器 (express router)，可以解決上述問題。

### 建立路由器

- 建立獨立文件，並建立路由器:

```js
const express = require("express");
const router = express.router();
```

- 原本使用 app 處理請求，改交由 router 管理:

```js
router.get("/register", (req, res) => {
  //
});
router.get("/login", (req, res) => {
  //
});
```

- 匯出路由器:

```js
module.exports = router;
```

### 註冊路由器

在 app 註冊路由器:

```js
const myRouter = require("./routes/myRouter");
app.use(myRouter);
```

### 在處理路由器前統一執行某程式

我們可以在註冊路由器前，傳入請求處理函數，Express 會在處理路由器前統一先執行該程式。

和處理多支請求函數類似，需調用 next() 函數:

```js
app.use((req, res, next) => {
  const login = true;
  if (login) {
    next();
  } else {
    res.redirect("/register");
  }
}, passportRouter);
```
