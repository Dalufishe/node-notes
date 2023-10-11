# 返回 html 內容及文件

Web 開發中，伺服端返回最核心的資料莫過於 html 文本內容。

### 返回 html 文本內容

在 Nodejs 伺服器對 res.write() 或 res.end() 寫入字串，其中包含 html 內容即可響應 html 文本內容。

```js
const server = http.createServer((req, res) => {
  res.end(`<h1>Hello</h1>`);
});
```

若要返回特定字符集編碼文本內容，須按照 http 協議，在 headers 加上 Content-Type 屬性 (不局限於 html)。

```js
const server = http.createServer((req, res) => {
  res.setHeader("Content-Type", "text/html;charset=utf-8");
  res.end(`<h1>你好</h1>`);
});
```

### 返回 html 文件

搭配 fs 檔案系統可實現返回 html 文件。

```js
const server = http.createServer((req, res) => {
  const html = fs.readFileSync(
    path.join(process.cwd(), "assets", "index.html"),
    "utf-8"
  );

  res.end(html);
});
```

注意: 若在 html 指定 charset 字符集編碼，則不須再設置 headers。