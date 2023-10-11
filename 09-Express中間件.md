# Express 中間件

在 Express 中的伺服器是本質上就是由一系列的中間件 (middleware) 組成 (一個具有請求物件 (req)，回應物件 (res)，和觸發下步 (next) 的函數)，Express 應用程式是一系列的中介軟體函數呼叫。

```md
Express 伺服器接收請求 => 一系列的中間件函式呼叫，處理請求 => 回應客戶端
```

### 註冊中間件

在 Express 中，我們可以使用 app.use() / app.METHODS() / app.all() 註冊中間件。

#### app.use() 註冊泛用的中間件:

- 整個網站使用的中間件

```js
app.use((req, res) => {});
// 或
app.use("/", (req, res) => {});
```

- 特定路徑下使用的中間件:

```js
// /home, /home/1, /home/contact, 中間件都會被調用
app.use("/home", (req, res) => {});
```

#### app.all() 註冊精確路徑的中間件:

- 根目錄使用中間件

```js
app.all("/", (req, res) => {});
```

- /home 使用中間件

```js
app.all("/home", (req, res) => {});
```

- 和 app.use() 一樣的效果

```js
app.all("/home/*", (req, res) => {});
// 和下面一樣
app.use("/home", (req, res) => {});
```

#### app.METHODS() 註冊特定請求的中間件:

app.METHODS() 和 app.all() 一樣，是精準的，多了可以處裡特定請求:

- get 方法

```js
app.get("/home", (req, res) => {});
```

- post 方法

```js
app.post("/register", (req, res) => {});
```

### Express 插件

在 Express 中，所謂的插件，就是別人寫好的中間件，僅此而已。

- bodyParser 用於處裡請求主體。

```js
app.use(bodyParser());
```
