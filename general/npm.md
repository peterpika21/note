###### tags: `通用技術`

# NPM

| 指令 | 說明 |
| --- | --- |
| npm install --save 套件名稱 | 安裝套件 |
| npm i | 安裝 package.json 內套件 |
| npm uninstall --save 套件名稱 | 移除套件 |
| npm uadate | 更新套件 |
| npm ls | 列出專案套件清單 |
| npm search 套件名稱 | 搜尋套件 |
| npm init | 建立 package.json |

```text
例如某套件 "express": "4.17.1"
主要版本為 4  次要版本為17  bug修正數為1

"express": "^4.17.1" 符號 ^ 為安裝 1.x.x  (x代表自動更新)
"express": "~4.17.1" 符號 ~ 為安裝 1.0.x  (x代表自動更新)
"express": "latest"  為安裝最新版本 (很少人這樣寫 程式容易壞掉)
```

