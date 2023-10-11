# Node 簡介

Node.js 是基於 Chrome V8 Javascript 引擎的 Javascript 運行環境。
Node.js 的出現使開發者可以在非瀏覽器的環境下運行 Javascript (Javascript 從此成為通用程式語言)，使伺服器開發、桌面開發成為可能。

作為前端開發人員，使用 Nodejs 學習後端是最低成本的方式之一，前後端開發環境的統一使得我們不必再熟悉一門新的程式語言就可以往後端邁進。

### Node 可以做什麼

- Web 伺服器
- 命令行工具
- 網路爬蟲
- 底層工具
- 桌面應用開發
- 嵌入式
- 還有更多！

### Node 學習路線

- Node 基礎概念
- Node 模組化規範
- Node 常用 API
- Node 建立 http 伺服器
- Express 框架
- Expresrs 中間件
- Express 靜態資源
- Express 請求參數、主體解析
- Express 視圖、模板引擎
- Express 路由器
- Express cookie 和 session
- MySQL 資料庫管理系統
- Node 操作 MySQL
- ORM 操作資料庫
- CSRF 跨站請求防護

### 安裝 Node.js

到 Node 官網下載 LTS 版本:

Node 官網: <a href="https://nodejs/org/en/">https://nodejs/org/en/</a>

Nodejs 安裝自帶 Nodejs 及 npm 包管理工具。

### 使用 Node 執行 Js 程式

1. 使用命令行工具開啟交互模式 (repl, 通常用於快速驗證某個結論):

```bash
node
Welcome to Node.js v18.16.0.
Type ".help" for more information.
>
```

2. 解釋 Javascript 文件 (實際開發):

```bash
node helloworld.js
```

### 安裝與使用 nvm

nvm (node version manager)，設計來管理多個 Nodejs 版本。

- 安裝與使用:

```bash
nvm install 版本號
nvn uninstall 版本號
nvm use 版本號
```

- 查看 node 版本列表:

```bash
nvm list

  * 18.16.0 (Currently using 64-bit executable)
    16.17.1
    10.15.0

```

### Nodejs 注意事項

Nodejs 作為非瀏覽器端的 Javascript 執行環境，其本身只能解釋 ECMAScript 語法 & 核心 API，以及 Nodejs API (如檔案讀寫、http)。瀏覽器上一些 API 像是 DOM 操作、window API 都是不存在的，試圖執行會報錯。
