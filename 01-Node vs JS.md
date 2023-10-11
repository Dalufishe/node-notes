# 一些 Node 和 瀏覽器 JS 的差異

### global 全局物件

在瀏覽器 JavaScript 中，有一個特殊的 window 物件，他的所有屬性都可在程序中直接被訪問，稱為全局變數。

Nodejs 也有個全局物件稱作 global，所有的全局變數 (內建) 都是 global 物件的屬性。

下列這些都是存儲在 global 中的全局變數:

```bash
console
setTimeout
process
```

#### global 注意事項

1. 交互模式下 (repl)，this 指向等於 global 物件，然而，執行 js 文件的情況下，this 不指向 global 物件，而是該模組 (僅限 commonjs)。

2. 在 Node 中聲明的變數並不會被放入 global 物件 (模組化緣故)。

```js
let a = 3;
console.log(this.a); // undefined
```

### Node.js 模組化

將系統拆分成子系統，彼此獨立、存在依賴相互引用，提供了代碼維護性、更好的風格及解決命名衝突。

在 Nodejs 中，一個文件表示一個模組，模組的作用域是私有的，模組間保持獨立，並且可以相互引用。

#### Commonjs & ES6

Node 存在 Commonjs (舊) 及 ES6 (新) 兩種模組化規範:

##### Commonjs

Commonjs 模組化使用 require & exports 表示匯入匯出。

- 第一種導入 / 導出數據:

```js
exports.value = 3;
```

```js
const m1 = require("./path/m1.js");
```

- 第二種導入 / 導出數據:

```js
module.exports = {
  value: 3,
};
```

```js
const m1 = require("./path/m1.js");
```

Commonjs 模組化的本質是往 exports 物件添加屬性或賦值，使其可被導入者訪問。

##### ES6

ES6 模組化使用 import & export 表示匯入匯出。

和 Web 寫法一致。

#### 模組中的 this 指向

使用 Commonjs 模組化方式，this 指向該模組 (exports)。

```js
this === exports && this === module.exports && exports === module.exports; // true
```

使用 ES6 模組化方式，this 值為 undefined。

#### 模組分類

一般項目中模組分為三種，Nodejs 內置模組、自己編寫的模組、以及第三方模組 (統一使用如 npm 包管理器管理)。

常見的內置模組:

- fs 檔案系統
- http 網路請求
- path 路徑操作
- querystring 查詢參數解析
- url 網址解析

