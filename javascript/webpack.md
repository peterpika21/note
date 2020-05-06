###### tags: `Javascript`

# webpack

## 簡介

![](https://i.imgur.com/dOFIbQD.png)

Webpack 與 gulp ，都能把我們撰寫的檔案編譯成瀏覽器看得懂檔案，整合、壓縮等等

:::info
gulp 與 webpack

gulp：前端自動化工具，可單獨使用單一任務，定義任務相對簡單，較為彈性，不需要進入點，雖然也能做模組化但沒 webpack 那麼優秀。

Webpack：模組打包工具，相依性很高的模組化，適合做 SPA，需要 SEO 時採用 SSR，需要一個 entry \(進入點\)
:::



## 安裝與基本教學

```javascript
npm init
npm install webpack webpack-cli --save-dev
```

package.josn 內容

```javascript
//npm指令
"svripts":{ 
  "build": "webpack"
}

//相依套件 
"dependencies":{ 
  "webpack": "^4.39.2",
  "webpack-cli": "^3.3.6"
}
```

建立 webpack.config.js

```javascript
const path = require('path')

module.exports = {
  
  entry: './src/main.js', // 進入點
  
  // 輸出到 dist 資料夾的 main.bundle.js 檔案
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.bundle.js'
  }
```

引入 JS 不需要套件 ，但其它檔案需要安裝各自的 loader

例如：引入 css 檔案

```javascript
npm install --save-dev css-loader //install css-loader
npm install --save-dev style-loader //install style-loader
```

```javascript
// webpack.config.js 加上模組

modele.exports = {
  module: { //new module 功能
    rules: [ //規則
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'], //install style-loader
      },
    ],
  },
}
```

[詳細中文文檔](https://www.webpackjs.com/concepts/)
