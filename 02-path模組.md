# path 模組

Node 內置模組之一，用於處理路徑操作。

```js
const path = require("path");
```

### 先備知識: \_\_dirname & \_\_filename

Nodejs 提供兩個內置變數，\_\_dirname 及 \_\_filename

- \_\_dirname 表示當前所在目錄 (絕對路徑)。
- \_\_filename 表示當前所在文件 (絕對路徑)。

嘗試看看:

```js
console.log({
  __dirname,
  __filename,
});
```

注意: 兩種內置變數只能在文件中使用。

### 獲取路徑資訊

path.basename(): 獲取一路徑的文件名。
path.extname(): 獲取一路徑的後綴名。
path.dirname(): 獲取一路徑的目錄名。

```js
console.log({
  後綴名: path.extname(__filename),
  文件名: path.basename(__filename),
  資料夾名: path.dirname(__filename),
});
```

path.parse(): 綜合方法，可獲取到文件、後綴、硬盤等資訊。

```js
console.log(path.parse(__filename));
```

```bash
{
  root: 'C:\\',
  dir: 'C:\\Users\\user\\Desktop\\Dalufishe\\Program\\Practice\\NodeJS\\new',
  base: 'test.js',
  ext: '.js',
  name: 'test'
}
```

### 拼接路徑

path.join() 設計來做路徑拼接。

```js
path.join(__dirname, "modules", "m1.js");
```

path.join() 提供了一種更優雅的方式做拼接，並對跨平台差異做處理 (斜線，反斜線)。
