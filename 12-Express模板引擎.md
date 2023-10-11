# Express 模板引擎

想在伺服端操作動態 html 頁面，我們可以透過模板技術，撰寫模板，並透過程式解析，達到注入動態內容的效果。

### art 模板引擎

我們選用 art 模板引擎:

- 安裝依賴

```bash
npm i art-template express-art-template
```

- 初始化 (註冊模板引擎、指定模板資料夾、使用模板)

```js
app.engine("art", require("express-art-template"));
app.set("view options", {
  debug: process.env.NODE_ENV !== "production",
});
app.set("views", path.join(process.cwd(), "views"));
app.set("view engine", "art");
```

### 向模板傳遞數據，並返回解析過後的頁面

透過 res.render() 方法，我們向前端返回解析過的 html 頁面。

render() 方法的第二個參數可以向模板注入數據。

```js
// 假數據，此數據可能動態來自資料庫
const data = {
  name: "Dalufishe",
  age: 18,
};

app.get("/", (req, res) => {
  res.render("index", data);
});
```

### 模板語法

使用雙大括號 `{{}}` 取得數據。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>首頁</h1>
    <ul>
      <li>Name: {{name}}</li>
      <li>Age: {{age}}</li>
    </ul>
  </body>
</html>
```

其他詳細請參照文檔: https://aui.github.io/art-template/
