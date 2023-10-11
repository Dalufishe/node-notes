# Express 靜態資源

express 提供了種內置插件 express.static() 處理靜態資源。
有了 express 靜態資源插件，我們不必再手寫靜態資源路由邏輯。

### 註冊插件

註冊插件，並指定靜態資源資料夾:

```js
app.use(express.static("public"));
```

### 配置靜態資源資料夾

```
- public
  - home.html
  - home.css
```

### 前端發送請求

在之後，只要前端發送匹配靜態資源資料夾 (public) 內的內容，則會返回對應資源。

```js
curl localhost:8080/home.html
```

```
返回 public/home.html 文件
```