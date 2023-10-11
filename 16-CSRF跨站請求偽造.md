# CSRF 跨站請求偽造

跨站請求偽造 CSRF (Cross Site Request Forgery)，是一種通過瀏覽器在 http 上對 cookie 預設行為進行身分偽造的技術。攻擊者盜用了使用者的身分，以使用者的身分發送惡意請求。

正常使用 cookie 或 session 登入，用戶訪問 A 網站，http 會自動挾帶 cookie 到 A 網站伺服器，以進行授權操作。CSRF 透過架設第三方網站 B，誘導使用者在 B 網站內向 A 網站伺服器發送請求，瀏覽器則會將使用者在 A 網站的 Cookie 傳送到 A 網站伺服器中，使 B 網站開發者可以偽造使用者向 A 網站發送請求 (匯款等其他帳號操作)。

### CSRF 防護

1. 瀏覽器跨域防護:

瀏覽器跨域防護使 Javascript 腳本發送的 ajax 請求受到了阻擋。這時攻擊者無法對開啟跨域保護的網站進行攻擊。

2. X-CSRFTOKEN:

請求如 form action 不受跨域限制保護 (最常見的 CSRF 攻擊方式)。使用 X-CSRFTOKEN 防護的網站會在使用者訪問頁面返回 csrf_token cookie，並在需要辨別使用者的請求在 headers 中添加 X-CSRFTOKEN: csrf_token，網站伺服器會對 X-CSRFTOKEN 和 cookie 的值進行比較，若不一樣則視為非法用戶。(非法網站使用 form action 無法設置 headers，就算使用了沒有跨域防護的 ajax 也因 cookie 的同源政策無法取得 cookie 達到雙層防護效果)

- 訪問頁面返回 csrf_token cookie:

```js
app.get("/", (req, res) => {
  const csrf_token = nanoid();
  res.cookie("csrf_token", csrf_token);
});
```

- 發送需授權的請求:

```js
axios.post("/transfer", {
  data: ...,
  headers: { "X-CSRFToken": getCookie("csrf_token") },
});
```

- 驗證合法用戶:

```js
app.post("/transfer", (req, res) => {
  const cookie_csrf_token = req.cookie[csrf_token];
  const headers_csrf_token = req.headers["X-csrftoken"];
  if (cookie_csrf_token === headers_csrf_token) {
    // 通過
    res.send("CSRF 通過");
  } else {
    // 不通過
    res.send("CSRF 不通過");
    return;
  }
});
```

### 在 Express 使用鉤子函數實作 CSRF 防護

```js
const xsrfProtect = (req, res, next) => {
  if (!req.cookies["xsrf_token"]) {
    res.cookie("xsrf_token", nanoid());
  }

  if (true /* 那些請求需跨站保護 */) {
    if (
      req.cookies["xsrf_token"] &&
      req.headers["x-xsrftoken"] === req.cookies["xsrf_token"]
    ) {
      console.log("csrf 驗證通過");
    } else {
      console.log("csrf 驗證不通過");
      return;
    }
  }

  next();
};
```
