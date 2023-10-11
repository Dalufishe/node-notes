# Buffer 數據類型

早期瀏覽器上的 Javascript 並沒有定義一個專門處理二進制數據的數據類型。但是在 Nodejs 中，我們需要頻繁處理檔案讀寫流、緩衝區，因此 Node 定義了一個 Buffer 類，專門存放和處理二進制數據。

Buffer 就像一個 Array；其為一專門儲存二進制數據的容器，並允許做數據轉換 (如 ASCII)。

- 二進制轉文本:

```js
let buf1 = Buffer.from([97, 98, 99]);
console.log(buf1); // <Buffer 61 62 63>
console.log(buf1.toString()); // abc
```

- 文本轉二進制:

```js
let buf2 = Buffer.from("nodejs");
console.log(buf2); // <Buffer 6e 6f 64 65 6a 73>
console.log(buf.toJSON().data); // [ 110, 111, 100, 101 ]
```

- 創建容器並寫入數據:

```js
let buf3 = Buffer.alloc(10);
buf3.write("hello");
console.log(buf3); // <Buffer 68 65 6c 6c 6f 00 00 00 00 00>
```
