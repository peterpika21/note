###### tags: `Javascript`

# 前後端分離與 SPA & SSR 簡介

### 傳統方式

> 由後端保存 HTML 每當切換頁面時就傳回新的 HTML

![](https://i.imgur.com/EPwYevw.png)


### SPA \(Single Page Application\)

> SPA 稱為`單頁面應用`，相較於傳統方式，view 是直接由前端負責，操控資料 render 畫面，此時前後端已經分離開來了，因為畫面是由資料 render 出來的，所以 HTML 只有一個而且是空的，此情況不利於 SEO

![](https://i.imgur.com/XOJWAkg.png)

### SSR \(Server Side Rendering\)

> SSR 稱為`伺服器渲染`主要是解決 SPA 對於 SEO 不利的問題，簡單來看就是 SSR 一樣是用 JS 操控畫面，且在背後會把網頁內容給渲染出來，這樣爬蟲就能抓到資訊了

![](https://i.imgur.com/Sn0ltgd.png)
