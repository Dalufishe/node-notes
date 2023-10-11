# Express 簡介

Nodejs 中可透過 http 原生 api 模組實現 http 伺服器，然而，在實際開發中，我們會選用一個相對高級 (http 相對底層) 的框架，除了簡化 api 之外，框架往往提供許多功能，提高開發效率、及提供更專業的方式解決複雜的需求。

Express 框架是現今 Nodejs 生態最為成熟，穩定的 Web 伺服器框架，並提供了下列功能:

- 靜態資源服務
- 路由控制
- 模板解析支持
- 動態視圖
- 用戶會話
- CSRF 保護
- 錯誤控制器
- 訪問日誌
- 緩存
- 插件支持
- 更多...

### 建立 express 伺服器

- 安裝依賴:

```bash
npm i express
```

- 導入框架:

```js
const express = require("express");
```

- 建立 app 物件，並監聽端口:

```js
const app = express();
app.listen(8080);
```


### 處裡 get 請求

app.get() 方法可針對不同的路徑 (基本路由處理) 對 get 請求做處裡並響應。

- req 是請求物件 (處理請求相關)
- res 是響應物件 (處裡響應相關)

```js
app.get("/", (req, res) => {});

app.get("/detail", (req, res) => {});
```

### 回應前端請求

express 和 http 模組一樣具有 res.end() 和 res.write() 方法。框架在此之上提供 res.send() 和 res.json() 等系列方法，對 api 做更高級的封裝，例如可以傳入 json (原生的 end() 方法只能傳入 string 和 buffer 物件)，以及內置解決許多編碼，文件類型的問題。

```js
app.get("/", (req, res) => {
  res.send("Hello world");
});

app.get("/detail", (req, res) => {
  res.send({ a: 1, b: 2 });
});

app.get("/data", (req, res) => {
  res.json({ a: 1, b: 2 });
});
```

### 獲取請求參數

在 express 中，`req.query` 可直接獲取請求參數 (查詢物件)。

```js
app.get("/", (req, res) => {
  console.log(req.query);
  res.send("Hello world");
});
```

### 處裡 post 請求

app.post() 方法可針對不同的路徑 (基本路由處理) 對 post 請求做處裡並響應。

```js
app.post("/register", (req, res) => {});
```

### 獲取請求主體

在 Node http 模組我們使用 req.on("data") 獲取請求主體 (並使用 req.on("end") 操作結果):

```js
const server = http.createServer((req, res) => {
  let reqBody = "";

  req.on("data", (data) => {
    reqBody += data.toString();
  });

  req.on("end", () => {
    console.log(reqBody);
  });
});
```

在 express 中，我們可以借助 `body-parser` 模組並搭配 req.body 直接取得請求主體:

- 配置 body-parser 插件:

```js
const body_parser = require("body_parser");
// 註冊 body parser 插件
app.use(body_parser.urlencoded({ extended: false })); // 解析 urlencoded 格式，不開啟擴展
app.use(body_parser.json()); // 解析 json 格式
```

- 獲取請求主體:

```js
app.post("/", (req, res) => {
  console.log(req.body);
  res.send("Hello");
});
```

### 處理動態路徑

再 Express 中，我們可以透過 `:` 處理動態路徑，並透過 `req.params` 獲取路徑參數。

```js
app.get("/detail/:id", (req, res) => {
  console.log(req.params);
});
```

### 重定向

res.redirect() 用於實作路由重定向。

```js
app.get("/", (req, res) => {
  res.redirect("/home");
});
```

redirect 方法會返回 302 狀態碼，並對 Location 進行設置，實作伺服端重定向。

### 返回 html 頁面

在 http 模組 我們使用 fs 將頁面讀取並返回:

```js
const server = http.createServer((req, res) => {
  const html = fs.readFileSync(
    path.join(process.cwd(), "views", "index.html"),
    "utf-8"
  );

  res.end(html);
});
```

使用 express，我們可以配置視圖引擎 (view engine)，簡化邏輯:

```js
app.engine("html", require("express-art-template") /* 隨便一個模板引擎 */);
app.set("views", path.join(process.cwd(), "views"));
app.set("view engine", "html");

app.get("/", (req, res) => {
  res.render("index");
});
```

注意: 使用視圖引擎需借助隨意一個模板引擎的幫助來解析 html。

### 多支請求處理函數

有時候我們有註冊多支請求處理函數的需求 (分批處理)，可傳入多個回調函數:

- next() 表示執行下一隻回調，並可傳入數據。

```js
app.get(
  "/",
  function (req, res, next) {
    console.log("the response will be sent by the next function ...");
    next();
  },
  function (req, res) {
    res.send("Hello from B!");
  }
);
```

```js
const cb1 = function (req, res, next) {
  console.log("the response will be sent by the next function ...");
  next();
};

const cb2 = function (req, res) {
  res.send("Hello from B!");
};

app.get("/", [cb1, cb2]);
```

