# 搭建 http 伺服器

在 Nodejs 中，我們使用 http 模組創建伺服器程序並處裡 http 請求。

```js
const http = require("http");
```

### 創建 http 伺服器

http.createServer() 用於在 Nodejs 環境下架設 http 伺服器:

- 建立伺服器 & 處裡請求

傳入 createServer 的回調函式會在每一次前端發送請求被調用一次。

```js
const server = http.createServer((req, res) => {
  // 處裡請求
});
```

- 監聽端口號

```js
server.listen(8080, (err) => {
  // 監聽完成 & 失敗調用
});
```

### 回應前端請求

res 參數用於回應前端請求，其中存在 write 及 end 方法:

- write(): 表示向前端返回數據。
- end(): 表示終止對前端返回數據。

```js
const server = http.createServer((req, res) => {
  res.write("hello");
  res.write("world");
  res.end();
});
```

end() 可傳入最後一次響應的數據:

```js
const server = http.createServer((req, res) => {
  res.end("hello world");
});
```

### 獲取請求訊息

req 參數紀錄請求相關的訊息，如 url, headers 等資訊。

- req.url: 獲取請求路徑 (不包括 host)
- req.headers: 獲取請求頭 (紀錄 host, cookies 等資訊)
- req.method: 獲取請求方式 (get, post 等等)

```js
const server = http.createServer((req, res) => {
  console.log(req.url);
  console.log(req.headers);
  console.log(req.method);
});
```

#### 獲取請求參數

Q: 何謂請求參數?
A: 在請求路徑後方，形式為 `?id=3&page=4` 稱作請求參數 (querystring)。

搭配 url 模組使用可獲得請求參數 (被解析的):

```js
const http = require("http");
const url = require("url");

const server = http.createServer((req, res) => {
  const query = url.parse(req.url).query;
  console.log(query); // 被解析的 querystring
});
```

#### 獲取請求主體

如 post 請求，請求挾帶了 post 請求主體。

獲取請求主體:

- req.on("data"): 獲取請求主體，流形式 (buffer)。
- req.on("end"): 流結束 (主體獲取完成) 執行。

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
