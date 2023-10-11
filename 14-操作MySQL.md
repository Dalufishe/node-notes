# 操作 MySQL

我們可以在 Nodejs 中操作 MySQL 語句。

### 安裝依賴

`mysql` 是一基於 MySQL 協議和 MySQL 服務連接、執行 MySQL 語句的第三方 Nodejs 庫。

```bash
npm i mysql
```

### 連接數據庫

`createPool` 函數用於建立連接池，我們可以使用連接池向數據庫建立連接 (`pool.getConnection`) ，並對數據庫做操作 (`connection.query`) 。

```js
const mysql = require("mysql");

// 創建連接池
const pool = mysql.createPool({
  host: "localhost",
  port: 3306,
  user: "root",
  password: "your_password",
  database: "world",
});

// 建立連接並操作數據庫
pool.getConnection((err, connection) => {
  connection.query("SELECT * FROM country", (err, rows) => {
    console.log(rows); // 查詢結果
    connection.release();
  });
});
```

### 封裝 sql 執行函式

我們手動建立 `query` 查詢函式，簡化查詢操作。

```js
function query(sql, callback) {
  pool.getConnection((err, connection) => {
    connection.query(sql, (err, result) => {
      callback(err, result);
      connection.release();
    });
  });
}
```

### 操作數據庫並返回

以下是數據庫、API 伺服器、前端運作流程的一個小範例。

伺服端程式碼:

```js
app.get("/get_data", (req, res) => {
  const { limit } = req.query;
  query(
    `SELECT 
            name, population 
        FROM 
            country 
        ORDER BY
            population 
        DESC
        LIMIT ${limit ? limit : 1}`,
    (err, result) => {
      res.send(result);
    }
  );
});
```

前端發送請求:

```bash
curl http://127.0.0.1:8080/get_data?limit=10
```

前端返回數據:

```bash
[
    {
        "name": "China",
        "population": 1277558000
    },
    {
        "name": "India",
        "population": 1013662000
    },
    {
        "name": "United States",
        "population": 278357000
    },
    {
        "name": "Indonesia",
        "population": 212107000
    },
    {
        "name": "Brazil",
        "population": 170115000
    },
    {
        "name": "Pakistan",
        "population": 156483000
    },
    {
        "name": "Russian Federation",
        "population": 146934000
    },
    {
        "name": "Bangladesh",
        "population": 129155000
    },
    {
        "name": "Japan",
        "population": 126714000
    },
    {
        "name": "Nigeria",
        "population": 111506000
    }
]
```
