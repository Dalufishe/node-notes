# http 路由處理

實際開發中，我們經常會根據前端對不同的路由發送請求，伺服端做不同的處理並響應。

### 判斷 req.url

透過判斷 `req.url` 對不同路由做處理，並返回內容。

- 返回不同的 html 文件:

```js
const server = http.createServer((req, res) => {
  const route = req.url;

  if (route === "/") {
    const htmlPath = path.join(process.cwd(), "assets", "index.html");
    const html = fs.readFileSync(htmlPath, "utf-8");
    res.end(html);
  } else if (route === "/detail") {
    const htmlPath = path.join(process.cwd(), "assets", "detail.html");
    const html = fs.readFileSync(htmlPath, "utf-8");
    res.end(html);
  }
});
```

- 處理 css 資源:

```js
const server = http.createServer((req, res) => {
  const route = req.url;

  if (route === "/") {
    const htmlPath = path.join(process.cwd(), "assets", "index.html");
    const html = fs.readFileSync(htmlPath, "utf-8");
    res.end(html);
  } else if (route.endsWith(".css")) {
    const cssPath = path.join(process.cwd(), "assets", route);
    let css = fs.readFileSync(cssPath);
    res.end(css);
  }
});
```
