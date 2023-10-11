# Cookie 和 Session

在日常開發中，伺服端需要辨識客戶端，以返回針對特定使用者的內容。透過傳遞身分驗證的資訊 (如帳號、密碼)，我們可以使伺服端能做到這件事。然而，要使用者在每一次請求都輸入身分驗證資訊是不實際的，因此我們需要一種方式存儲使用者狀態，使伺服器可以`持續性`的辨別客戶端 (登入)。

http 是一種無狀態協議，請求間是無狀態的 (單純使用 http 是無法持續辨識使用者)。

### Cookie

Cookie 是存儲在瀏覽器上的一小端文本信息。我們使用 Cookie 實作登入系統，核心概念就是將此段信息持續地發送至伺服器，以達到使用者持續辨識的效果。

為甚麼是 cookie ?

瀏覽器上能存儲數據除了 cookie 還有 localstorage 等方式。一般情況下之所以使用 cookie，在於 cookie 一系列的安全性防護，以及便利性有關。

- cookie 可由伺服器生成並返回，並設置成不可被腳本操作。
- cookie 有過期時間，默認關閉瀏覽器過期。
- http 請求會自動挾帶該網站的 cookie (便利性)。
- cookie 基於域名安全。

cookie 被設計成高度貼和 http，一來在於便利性，二來在於安全性的保障。

#### 使用 Cookie 保存狀態之交互流程

- 第一次訪問網站或登入，伺服器會返回 cookie 保存用戶信息。
- 瀏覽器自動將 cookie 訊息保存在瀏覽器中。
- 第二次以後請求，http 會挾帶 cookie 資訊 (存儲在 header 中) ，達成伺服端持續性的辨識使用者。

#### 使用 Express 操作 Cookie

- 安裝依賴 cookie-parser

```bash
npm i cookie-parser
```

- 引入並註冊 cookie-parser

```js
const cookieParser = require("cookie-parser");
app.use(cookieParser());
```

- 訪客模式下 (訪問網站即設置 cookie):

```js
app.get("/", (req, res) => {
  if (!req.cookies["name"]) {
    // 第一次訪問網站 (cookie 為空)，則為訪客返回一個隨機生成名字，並存儲在 cookie
    res.cookie("name", `${Math.random() + new Date()}`, {
      maxAge: 1000 * 60 * 60 * 24 * 3,
      httpOnly: true,
    });
  }
  // 之後訪問網站，伺服端則可辨識該訪客!
  else {
    console.log(req.cookies["name"]);
  }

  res.send("Hello Cookie!");
});
```

- 登入下 (登入後設置 cookie):

```js
app.post("/login", (req, res) => {
  const { username, password } = req.body;

  // 登入時，將帳號密碼等使用者狀態存儲在 cookie
  res.cookie("username", `${username}`, {
    maxAge: 1000 * 60 * 60 * 24 * 3, // 過期時間
    httpOnly: true, // 設置成不可被前端腳本操作
  });

  res.cookie("password", `${password}`, {
    maxAge: 1000 * 60 * 60 * 24 * 3,
    httpOnly: true,
  });

  res.send("Hello Cookie!");
});

app.get("/", (req, res) => {
  // 若使用者登入，伺服端則可辨識使用者
  console.log({
    username: req.cookies["username"],
    password: req.cookies["password"],
  });
});
```

### Session

Session 表示持續性的狀態存儲，這裡指儲存在伺服端的使用者狀態。

他被視作 Cookie 存儲狀態的替代品，相較於將狀態暴露的存儲在 Cookie，使用 sessionId (sid) 並搭配伺服端 Session 在保障安全性的前提下，達到持續性辨識使用者的效果。

#### cookie vs session

- 傳統使用 cookie

使用者狀態直接存儲在客戶端 (cookie 中)，過於暴露:

```
Cookie: username=Dalufishe;password=1341348193481
```

- 使用 session & sid

Cookie 僅存儲對應到 sid (不具備可讀性)，實際用戶狀態存在後端 session 中。

```
Cookie: sid=fjdakv8293ifjv823u1i4109fdjk
```

```js
Session = {
  fjdakv8293ifjv823u1i4109fdjk: {
    username: "Dalufishe",
    password: "1341348193481",
  },
};
```

#### 使用 Session 保存狀態之交互流程

- 第一次登入網站，伺服器會將使用者狀態發送至伺服器，存儲於 Session，並返回 sid 保存用戶信息。
- 瀏覽器自動將 cookie 訊息 (sid) 保存在瀏覽器中。
- 第二次以後請求，http 會挾帶 cookie 資訊 (sid) (存儲在 header 中)，並在伺服端轉換成使用者狀態 (可能是帳號、密碼) 達成伺服端持續性的辨識使用者。

#### 使用 Express 操作 Session

- 安裝依賴 cookie-session

```bash
npm i cookie-session
```

- 引入並註冊 cookie-session

```js
const cookieSession = require("cookie-session");
app.use(
  cookieSession({
    name: "my_session", //session 名稱
    keys: ["##@@##@@adfdadfdafewa32$j4kj2"], // sid 加密公式依賴的餐數 (隨便打即可)
    maxAge: 1000 * 60 * 60 * 24 * 3, // sid 過期時間
    httpOnly: true, // sid 不可被前端腳本操作 (預設為 true)
  })
);
```

- 登入下使用 session

```js
app.get("/login", (req, res) => {
  const { username, password } = req.query;
  req.session["username"] = username;
  req.session["password"] = password;
  res.redirect("/");
});

app.get("/", (req, res) => {
  console.log(req.session);
  res.send("Hello");
});
```
