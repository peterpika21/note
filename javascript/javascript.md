###### tags: `Javascript`

# JS常用技術

## 事件處理

### 事件處理模型

#### 內聯模型 \(不推薦\)

```markup
<button onclick="alert('hello')"> say hello </button>
```

#### 傳統模型 \(不推薦\)

```javascript
document.getElementById('myBtn').onclick = function(){
    alert('hello')
}
```

#### DOM Level 2 \(事件處理函式\)

```javascript
/* 
    addEventListener(event,function [,useCapture])
    event: 事件類型
    function: 執行函式
    useCapture: 選擇性參數(布林值), true 代表事件捕捉 , false 代表事件冒泡(預設)
*/

const el = document.getElementById('myBtn')

el.addEventListener('click',function(){
    alert('hello')
},false)
```

### Event 物件

**屬性**

| 名稱 | 說明 |
| :--- | :--- |
| currentTarget | 目前的事件對象 |
| target | 分派事件的原始對象 |
| type | 事件的類型 |
| bubbles | 冒泡狀態，布林值，true 代表會在 DOM 中往上冒泡 |
| cancelable | 事件是否為可取消的，布林值 |

**方法**

| 名稱 | 說明 |
| :--- | :--- |
| preventDefault\(\) | 取消事件的行為，但無法阻止事件的傳播，如取消 &lt;a&gt; 連結的點選效果 |
| stopPropagation\(\) | 停止事件的傳播 |

### EventTarget 物件

**方法**

| 名稱 | 說明 |
| :--- | :--- |
| addEventListener | 從事件對象上加入監聽 |
| removeEventListener | 從事件對象上移除監聽 |
| dispatchEvent | 送出事件給所有有訂閱的監聽者 |

**事件捕捉\(capturing\):** 由最外層往內找到指定元素

**事件冒泡\(bubbling\):** 由指定元素往外找

![](https://i.imgur.com/CdspPu6.png)

## Ajax

Ajax 是 Asynchronous JavaScript And XML 的簡寫，代表 Ajax 具有非同步、使用 JavaScript 與 XML 等技術的特性

### XMLHttpRequest 物件使用

```javascript
let xhr = new XMLHttpRequest();
xhr.open('get',url,true) // true為非同步

// 若使用 post , open 之後需給 type 如:
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded")

xhr.send(nul)

// 非同步資料取得後執行
xhr.onload = function(){
    ...
}
```

:::info
開發時請直接使用相關 plugin 如: axios
:::


### 跨來源資源共用 \( CORS \)

> 跨來源資源共用（Cross-Origin Resource Sharing \(CORS\)）是一種使用額外 HTTP 標頭令目前瀏覽網站的使用者代理取得存取其他來源（網域）伺服器特定資源權限的機制。當使用者代理請求一個不是目前文件來源——例如來自於不同網域（domain）、通訊協定（protocol）或通訊埠（port）的資源時，會建立一個跨來源 HTTP 請求（cross-origin HTTP request）。

要在不同網域傳送資訊

Server 的 Response Header 必須加上

Access-Control-Allow-Origin: \*

## 非同步處理

### Callback function

> 回呼函式（callback function）是指能藉由參數（argument）通往另一個函式的函式。它會在外部函式內調用、以完成某些事情，但使用太多則會造成回呼地獄

```javascript
function greeting(name) {
  alert('Hello ' + name);
}

function processUserInput(callback) {
  var name = prompt('輸入你的名字：');
  callback(name);
}

processUserInput(greeting);
```

### Promise

> promise 能解決 callback function 巢狀問題，且 promise 可以串接

```javascript
function a(data){
  return new Promise((resolve, reject) =>{
    return resolve(data)
  })
} 

function b(data){
  return new Promise((resolve, reject) =>{
    return resolve(data)
  })
} 

a('1')
  .then(res=>{
  // 處理
  return b(res) // 呼叫 B promise
})
  .then(res=>{
  // 處理
})
  .catch(err=>{
  // 錯誤處理
})
```

#### promise.all

> 可以同時呼叫多個 promise 回傳一個包含結果的陣列

```javascript
function a(data){
  return new Promise((resolve, reject) =>{
    return resolve(data)
  })
} 

function b(data){
  return new Promise((resolve, reject) =>{
    return resolve(data)
  })
} 

Promise.all([a('1'),b('2')]).then(res=>{
  console.log(res) // 回傳 ["1","2"]
})
```

### Async / Await

> Async / Await 的底還是 promise ，但又更精簡了寫法

```javascript
async function demo(){
    await promiseA;
// ... 等取得 promiseA 後做些什麼
    await promiseB;
// ... 等取得 promiseB 後做些什麼
}
```

#### 實務上範例

```javascript
async demo() {
  try {
    await promiseA
      .then(res => { 
        if (xxx) { // 如果有錯誤
          throw '發生錯誤' // 拋出錯誤訊息
        }
        //處理
      })

    // 等取得 promiseA 後進行接下來的處理
  } catch (error) {
    // 錯誤處理
  }
}
```

## 閉包 \(Closure\)

簡單來說，就是呼叫函式內的函式，將記憶體封存在內層

```javascript
    function callMethod(newMoney) {
      var money = newMoney || 1000;

      return function(price) {
        money -= price;
        return money;
      };
    }

    let myMoney = callMethod(3000);
    console.log(myMoney(100));
    console.log(myMoney(100));
    console.log(myMoney(100));
    
    結果：
    2900
    2800
    2700
```

## 展開與其餘

**合併**

```javascript
    let groupA = ["a", "b", "c"];
    let groupB = ["d", "e"];
    let groupAll = [...groupA, ...groupB];
```

**類陣列**

```javascript
若 body 內有一群 li 元素
let doms = document.querySelectorAll("li");
// 此時 doms 很像陣列但不是一個真正的陣列,會少很多方法

使用展開把類陣列轉成陣列
let newDoms = [...doms]


// 另一個例子 arguments 接收傳來的參數
(function demo() {
  console.log(arguments);
})(1, 2, 3, 4, 5, 6, 7);

// arguments 也是類陣列,能用展開轉換成陣列
let arg = [...arguments]
```

**其餘參數**

```javascript
(function demo(...money) {
  console.log(money);
})(1, 2, 3);

結果:
[1,2,3]
```

## 解構

### 基本

```javascript
    let demo = [1, 2, 3];
    [a, , b] = demo;
    結果:
    a=1
    b=3
    
    // 也可用於字串
    let str = "哈囉妳好嗎";
    [a, b] = str;
    a='哈'
    b='囉'
```

### 交換變數

```javascript
    let a = 1;
    let b = 2;
    [a, b] = [b, a];
```

### 物件的解構

```javascript
    let obj = {
      a: 1,
      b: 2,
      c: 3
    };
    
    let { a } = obj;
    結果:
    a=1
    
    // 也可以重新賦予變數名稱
    let { a:d } = obj;
    結果:
    d=1
```

### 預設值

```javascript
let [a = 1, b = 2] = [3];
結果:
a=3
b=2
```

## 縮寫

**物件的縮寫**

```javascript
let obj = {
  a: 1
};
let b = 2;

那麼 {obj:obj, b:b} 可以縮寫成 {obj,b}

在 Vue 裡面很容易看到這種寫法，例如

import Vue from 'vue'
import App from './App'
import router from './router'

new Vue({
  el: '#app',
  router, // 使用縮寫, 其實是 router:router
  template: '<App>',
  components: { App }
});
```

**物件的合併**

```javascript
let obj1 = {
  a: 1,
  h: 2
};

let obj2 = {
  a: 3,
  b: 4,
  c: 5
};

let obj3 = { ...obj1, ...obj2 };

結果:
{a:3,h:2,b:4,c:5}
```

## Class
ES6語法，屬於讓原型寫法更清晰的語法糖

### 傳統 prototype

```javascript
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

### class 基本用法

```javascript
// 定義一個類
class Point {
  // 建構式
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  // 方法
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

// 使用
let point1 = new Point(1,2)
let point2 = new Point(3,4)
```

### getter 與 setter \(不推薦\)

在比較複雜的情況，可以使用 get 與 set，基本用法如下

```javascript
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

### static

類相當於實例的原型，所有在類中定義的方法，都會被實例繼承。如果在一個方法前，加上`static`關鍵字，就表示該方法不會被實例繼承，而是直接通過類來調用，這就稱為**靜態方法**

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

#### 父類的靜態方法，可以被子類繼承

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello'
```

### 私有屬性與方法

以`#`開頭當作私有屬性或方法，外部無法讀取

```javascript
class Point {
  // 定義私有屬性
  #x;

  constructor(x = 0) {
    this.#x = +x;
  }

  get x() {
    return this.#x;
  }

  set x(value) {
    this.#x = +value;
  }
}
```

### 繼承

使用`extends`繼承父類

```javascript
class Point {
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 調用父類的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 調用父類的toString()
  }
}
```

## 防抖與節流
防抖與節流能針對觸發非常頻繁的函式做效能優化

### 防抖 \(debounce\)

> 在事件觸發 n 秒後再執行回調，如果在這 n 秒內又被觸發，則重新計時

```javascript
function debounce(fn, wait) {
    let callback = fn;    
    let timerId = null;

    // 原理就是在閉包內維護一個定時器
    return function() {
        // 保存作用域
        let context = this;
        // 保存參數
        let args = arguments;        

        clearTimeout(timerId);        
        timerId = setTimeout(function() {            
            callback.apply(context, args);
        }, wait);
    } 
}

// test
let resizeFun = function(e) {
    console.log('resize');
};

// 改變畫面寬度時觸發
window.addEventListener('resize', debounce(resizeFun, 500));
```

#### 應用場景

1. search時，用戶不斷輸入資料，用防抖來節約請求資源
2. windoow 觸發 resize 時，不斷的調整瀏覽器窗口大小會不斷的觸發事件，用防抖來讓其觸發一次

### 節流 \(throttle\)

> 在單位時間內，只會觸發一次函式

```javascript
function throttle(fn, wait) {
    let callback = fn;    
    let timerId = null;

    // 是否第一次執行
    let firstInvoke = true;

    return function() {
        let context = this;
        let args = arguments;           

        // 如果是第一次觸發，直接执行
        if (firstInvoke) {
            callback.apply(context, args);
            firstInvoke = false;
            return ;
        }

        // 如果定時器，直接返回        
        if (timerId) {
            return ;
        }

        timerId = setTimeout(function() {  
            // 注意 clearTimeout 放到內部執行了
            clearTimeout(timerId);
            timerId = null;

            callback.apply(context, args);
        }, wait);
    }
}

// test
let resizeFun = function(e) {
    console.log('resize');
};
window.addEventListener('resize', throttle(resizeFun, 500));
```

#### 應用場景

1. 鼠標不斷點擊觸發
2. 監聽滾動事件


## 瀏覽器存取
瀏覽器存取資訊方式，記住別存取敏感資料

### 比較

![](https://i.imgur.com/rUeaEcZ.png)

### 語法

#### localStorage 與 sessionStorage

> 資料是以類似 JSON 的 Key-value pair 格式儲存 , key 與 value 都是字串

```javascript
因為格式需要字串，設定與取得請善用 JSON.stringify() 與 JSON.parse()

// 設定
localStorage.setItem(key, value)
sessionStorage.setItem(key, value)

// 取得
localStorage.getItem(key)
sessionStorage.getItem(key)

// 移除
localStorage.removeItem(key)
sessionStorage.removeItem(key)
```

#### cookies 請直接用網路上的 pulgin

### 應用場景

| 名稱 | 場景 |
| --- | --- |
| cookies | 通常應用在儲存 token |
| localStorage | 購物車資訊 |
| sessionStorage | 頁面傳資訊 |


## Module
模塊可以拆分檔案，也能使用別人寫好的模塊

### require \(使用 CommonJS 規範\)

#### 輸出

```javascript
module.exports = {
  getText: function() {
    return 'hello';
  }
}
```

#### 接收

```javascript
var example = require("./example.js");
console.log(example.getText()); // 可使用載入的方法
```

### export \(ES6\)

> export 規定模塊的對外接口

```javascript
// 寫法1
export var m = 1;

// 寫法2
var m = 1;
export {m};

// 寫法3 可用 as 重新命名
var n = 1;
export {n as m};

// 寫法4 也能輸出函數
export function f() {};

// 寫法5 可以使用 export default 匿名方式
export default function () {}
```

#### 規則

```javascript
// error: 沒有指定對外接口
var m = 1;
export m;

// 需改成
var m = 1;
export {m};

// error: 沒有指定對外接口
export 42;

// 需改成
export default 42;

// error: default 已經是變量,不可再加變量
export default var a = 1;

// 需改成
var a = 1
export default a;
```

### import \(ES6\)

> import 規定模塊的載入接口

```javascript
// profile.js 輸出
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };

// main.js 載入
import { firstName, lastName, year } from './profile.js';

// 可以用 as 改名
import { firstName as newName, lastName, year } from './profile.js';

// 可使用 * 整體加載
import * as profile from './profile.js';
console.log(profile.year) // 1958
```

### 匿名用法

```javascript
// export-default.js
export default function () {
  console.log('foo');
}

// import-default.js
import customName from './export-default'; // 可自訂名稱
customName(); // 'foo'
```

### 複合寫法

```javascript
export { foo, bar } from 'my_module';

// 可以理解為
import { foo, bar } from 'my_module';
export { foo, bar };
```

## WebSocket
網路協定的一種，只要與伺服器連線一次就能保持通訊，常應用於聊天室

### 簡介

1. 請求網址通常類似 `ws://example.com`或經過 SSL 加密 `wss://example.com`
2. 優點：保持連線、較小的控制開銷、更好的壓縮效果
3. 若瀏覽器不支援，請找 Socket.io 庫

### server 端 \(使用 Node.js\)

```javascript
npm install express
npm install ws
```

#### server.js

```javascript
const express = require("express");
const SocketServer = require("ws").Server;

// 指定開啟的 port
const PORT = 3000;

// 創建 express 的物件, 並綁定及監聽 3000 port
const server = express().listen(PORT, () => console.log(`監聽在 ${PORT}`));

// 將 express 交給 SocketServer 開啟 WebSocket 的服務
const wss = new SocketServer({ server });

// 當 WebSocket 從外部連結時執行
wss.on("connection", ws => {
  console.log(`伺服端已連結`);

  // 對 message 設定監聽, 接收從 Client 發送的訊息
  ws.on("message", data => {
    // 取得所有連接中的 client
    let clients = wss.clients;

    // 做迴圈, 發送訊息至每個 client
    clients.forEach(client => {
      client.send(data);
    });
  });

  ws.on("close", () => {
    clearInterval(sendNowTime);
    console.log(`伺服端已中斷`);
  });
});

```

### client 端

#### index.html

```markup
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <input type="text" id="myInput" />
    <button id="myBtn">發送</button>
    <p>訊息清單</p>
    <ul id="myList"></ul>
    <script src="./index.js"></script>
  </body>
</html>

```

#### index.js

```javascript
let myInput = document.querySelector("#myInput");
let myBtn = document.querySelector("#myBtn");
let myList = document.querySelector("#myList");

// 使用 WebSocket 的網址向 Server 開啟連結
let ws = new WebSocket("ws://localhost:3000");

// 開始後執行的動作, 指定一個 function 會在連結 WebSocket 後執行
ws.onopen = () => {
  console.log(`客戶端已連接`);
};

// 關閉後執行的動作, 指定一個 function 會在連結中斷後執行
ws.onclose = () => {
  console.log(`客戶端已中斷`);
};

// 接受 server 發送的訊息
ws.onmessage = event => {
  // 伺服器回傳的消息
  let str = event.data;

  // 創建 li 節點
  let node = document.createElement("LI");
  let textnode = document.createTextNode(str);
  node.appendChild(textnode);
  myList.appendChild(node);
};

myBtn.addEventListener("click", function() {
  // 發送訊息
  ws.send(myInput.value);
  myInput.value = "";
});

```


## 正則表達式(RegExp)

### 屬性

| 名稱 | 說明 |
| :--- | :--- |
| global | 是否具有標誌 g |
| ignoreCase | 是否具有標誌 i |
| multline | 是否具有標誌 m |
| lastIndex | 標誌開始下一次匹配的字符位置 |
| source | 正則表達式的原文本 |

### 方法

| 名稱 | 說明 |
| :--- | :--- |
| test | 檢測字串中指定的值，返回布林值 |
| exec | 檢測字串中指定的值，返回找到的值，並確定位置 |
| compile | 編輯正則表達式 |

### 支持正則表達式的 String 物件方法

1. search
2. match
3. replace
4. split

### 語法

```javascript
let str = 'demo';
str = str.search(/字串/修飾符);

// 或是

let pattern = new RegExp(字串, 修飾符);
str = str.search(pattern);
```

### 修飾符

| 名稱 | 說明 |
| :--- | :--- |
| g | 全局匹配 |
| i | 大小寫都匹配 |
| m | 多行匹配 |

### 量詞

| 名稱 | 說明 |
| :--- | :--- |
| ^n | n 開頭的字串 |
| n$ | n 結尾的字串 |
| n\* | n出現0次以上 |
| n? | n出現0~1次 |
| n+ | n出現1次以上 |
| n{x} | n出現x次 |
| n{x,y} | n出現x~y次 |
| n{x,} | 出現x次以上 |
| n{,y} | 出現y次以下 |
| ?=n | 匹配任何緊接著 n 的字串 |
| ?!n | 匹配任何沒有緊接著 n 的字串 |

### 方括號

| 名稱 | 說明 |
| :--- | :--- |
| \[abc\] | 尋找括號間的任何字符 |
| \[^abc\] | 尋找不在括號間的字符 |
| \(red\|blue\|green\) | 查找任何指定選項 |

### 特殊符號

| 名稱 | 說明 |
| :--- | :--- |
| . | 任意字元 |
| \d | 任何數字，等於 \[0-9\] |
| \D | 任何非數字，等於 \[^0-9\] |
| \w | 任何數字字母底線，等同 \[A-Za-z0-9\_\] |
| \W | 任何非數字字母底線，等同 \[^A-Za-z0-9\_\] |
| \s | 任何空白字元，等同 \[ \f\n\r\t\v\] |
| \S | 任何非空白字元，等同 \[^ \f\n\r\t\v\] |


## Math

### 屬性

| 名稱 | 說明 |
| :--- | :--- |
| Math.E | 自然數 e = 2.71 |
| Math.LN2 | e 為底數的對數 2 = 0.69 |
| Math.LN10 | e 為底數的對數 10 = 2.30 |
| Math.LOG2E | 2為底數的對數 e = 1.44 |
| Math.LOG10E | 10為底數的對數 e = 0.43 |
| Math.PI | 圓周率 = 3.14 |
| Math.SQRT\_2 | 1/2的平方根 = 0.7 |
| Math.SQRT2 | 2 的平方根 = 1.414 |

### 方法

| 名稱 | 說明 |
| :--- | :--- |
| Math.abs\(num\) | 絕對值 |
| Math.ceil\(num\) | 無條件進位 |
| Math.floor\(num\) | 無條件捨去 |
| Math.max\(n1,n2\) | 取最大值 |
| Math.min\(n1,n2\) | 取最小值 |
| Math.pow\(n1,n2\) | n1 的 n2 次方 |
| Math.random\(\) | 0 ~ 1 的亂數 |
| Math.round\(num\) | 四捨五入 |
| Math.acos\(num\) | 反餘弦函數 |
| Math.asin\(num\) | 反正弦函數 |
| Math.atan\(num\) | 反正切函數 |
| Math.cos\(num\) | 餘弦函數 |
| Math.exp\(num\) | e 的 num 次方 |
| Math.log\(num\) | e 為底的對數 |
| Math.sin\(num\) | 正弦函數 |
| Math.sqrt\(num\) | 平方根 |
| Math.tan(num) | 正切函數 |

## 陣列 (Array)


### 參考特性 (重要)

```javascript
let arr = [1,2,3]

let demo = arr

demo.push(1) 

// 此時 arr 也會被影響

解決方案: 
demo = arr.slice()
或
demo = [...arr] // 使用展開
```

### 屬性

| 名稱 | 說明 |
| --- | --- |
| length | 長度 |

### 方法

| 名稱 | 說明 |
| --- | --- |
| concat\(arr\) | 合併陣列並回傳 |
| join\(str\) | 陣列轉字串並用 str 分隔，回傳 |
| unshift\(data\) | 陣列首加入 data |
| push\(data\) | 陣列尾加入 data |
| shift\(\) | 移除陣列第一個元素 |
| pop\(\) | 移除陣列最後一個元素 |
| splice\(index,num\) | 移除索引 index 開始的 num 個項目 |
| reverse\(\) | 將陣列順序顛倒 |
| slice\(start,end\) | 取 start 至 end - 1 的元素並回傳新陣列 |
| sort\(\) | 元素重新排序 \(由小到大\) |
| toString\(\) | 轉字串 |
| indexOf\(item\) | 尋找 item 的索引 |

###  迭代方法

| 名稱 | 說明 |
| :--- | :--- |
| forEach\(\) | 可代替 for 迴圈，注意迴圈內 if 無法使用 return 跳出 |
| map\(\) | 回傳新陣列，可迭代並操控值，如 arr.map\(e=&gt;e\*2\) ，陣列每個值乘2 |
| filter\(\) | 回傳新陣列，可篩選值，如 arr.filter\(e=&gt;e&gt;2\)，回傳值大於 2 者 |
| find\(\) | 類似 filter\(\) 但只會回傳第一筆找到的值 |
| every\(\) | 回傳布林值，全部結果相等 |
| some\(\) | 回傳布林值，部分結果相等 |
| reduce\(\) | 回傳新陣列，累加器，例子如下 |

```javascript
// reduce 非常好用,它能自訂累加器,做更複雜的變換, return 新的值 , 不會更改原本陣列

let newArr = arr.reduce((a,b)=>a+b) // 陣列數字相加
// 結果 6

也可自訂累加器格式,例如:

 let res = arr.reduce((a,b)=>{
   a[b] = b
   return a
 },{})
 
 // 結果 
 res = {
  1: 1,
  2: 2,
  3: 3
}
```

## 函式 \(function\)

### 傳統函式

```javascript
function name(參數){
    // 處理
    return // 回傳值, 可省略 ,回傳後就會跳出函式
}
```

### IIFE \(立即呼叫函式\)

```javascript
(function name(參數){
    // 處理
    return // 回傳值, 可省略 ,回傳後就會跳出函式
})()
```

### 箭頭函式 \(ES6\)

```javascript
function add(x){
    return x
}

轉成箭頭函式

let add = (x) => x
```

箭頭與傳統函式差異

1. 箭頭函式沒有 arguments 參數 , 需使用其餘參數
2. this 指向不同

```javascript
傳統函式的 this = 函式的呼叫方式

ex:
var name = '小白'

var obj = {
    name: '小黑',
    callName (){
        console.log(this.name) // 小黑
        setTimeout(function(){
            console.log(this.name) // 小白
        },10)
    }
}

物件內的 this 指向物件本身
但 setTimeout 的 this 則屬於 windows 下
我們把 callName 改寫

callName (){
    let vm = this
    setTimeout(function(){
        console.log(vm.name) // 小黑
    },10)
}

此時 this 就能指向物件內的 name
```

## Error 物件

### 錯誤類型

| 名稱 | 說明 |
| :--- | :--- |
| Error | 一般的錯誤情況，最常用在客製化例外情況 |
| RangeError | 發生在當數值落在特定區間外時，例如：透過 toFixed\(\) 方法時，它可以接受介於 0~20 的參數來說明要顯示到小數後第幾位，當超過這個區間時，就會拋出錯誤 |
| ReferenceError | 找不到變數，經常發生在拼錯字的情況 |
| SyntaxError | 語法錯誤，當有違反 javascript 語法規則時，則拋出錯誤 |
| TypeError | 如果某一變項的型別和所期待的操作不同時，則拋出錯誤，經常發生在呼叫一個不存在的函式時 |
| URIError | 當使用 encodeURI\(\) 或 decodeURI\(\) 的方法，但卻給了不合法的 URI |

###  錯誤處理

```javascript
try{
// 可能發生錯誤的敘述,若捕捉到錯誤立即進入 catch
}
catch(e)
{
 // 捕捉錯誤
}
finally
{
 // try...catch 後觸發,一定會執行
}
```

### catch 的屬性

| 名稱 | 說明 |
| :--- | :--- |
| number | 錯誤碼 |
| message | 錯誤訊息 |
| description | 錯誤描述 |

**客製化錯誤**

透過 **`throw`** 可以客製化錯誤訊息

```javascript
try{
  throw 'msg' // 拋出例外
  
  throw new RangeError('msg') // 也可以創建錯誤型別
}
```

### 客製化錯誤名稱

```javascript
/* 客製化錯誤類型 */
function DivisionByZeroError(message){
    this.name = 'DivsionByZeroError'
    this.message = message
}

/* 繼承 Error 物件 */
DivisionByZeroError.prototype = new Error()
DivisionByZeroError.prototype.constructor = DivisionByZeroError

/* 建立 showError 的方法 */
DivisionByZeroError.prototype.showError = function(){
    return this.name +': "' + this.message +'"'
}


/* 使用客製化錯誤類型 */
try{
    throw new DivisionByZeroError('msg')
}
catch(e){
    console.log(e.showError())
}
```

## 字串 (string)

### 基本

* 字串必須使用單引號或雙引號
* 某些字元須使用逸脫字元 \(escaped character\)

### 逸脫字元

| 名稱 | 說明 |
| :--- | :--- |
| \" | 雙引號 |
| \' | 單引號 |
| \\ | 反斜線 |
| \b | BackSpace |
| \f | 換行 \(Form Feed\) |
| \n | 換行 \(New Line\) |
| \r | 換行 \(Carriage Return\) |
| \t | Tab |
| \xNN | Latin-1 字元 \(NN 為十六進為表示法\) |
| \uNNNN | Unicode 字元 \(NNNN 為十六進為表示法\) |

### 屬性

| 名稱 | 說明 |
| :--- | :--- |
| length | 長度 |

### 方法

| 名稱 | 說明 |
| :--- | :--- |
| charAt\(index\) | 傳回字串中所引為 index 的字元，舉例來說，假設變數 X 的值為 "JavaScript程式設計"，則 X.charAt\(0\) 會傳回字元 J，X.charAt\(5\) 會傳回字元 c |
| charCodeAt\(index\) | 傳回字串中所引為 index 的字元字碼，舉例來說，假設變數 X 的值為 "JavaScript程式設計"，則 X.charCodeAt\(0\) 會傳回字元 J 的字碼 74 |
| indexOf\(str,start\) | 從索引為 start 處開始尋找子字串 str，找到則回傳索引，否則回傳 -1，若 start 省略不寫，就從頭開始尋找 |
| lastIndexOf\(str\) | 尋找子字串 str 最後的索引 |
| match\(str\) | 尋找子字串 str ，傳回值為字串，不是索引 |
| search\(str\) | 與 indexOf\(\) 相同 ，支援正則表達式，不可指定起始點 |
| concat\(str\) | 串接字串，假設變數 X 的值為 "JavaScript程式設計"，則 X.concat\("一級棒"\) 會傳回 "JavaScript程式設計一級棒" |
| replace\(str1,str2\) | 將尋找到的子字串 str1 取代為 str2，支援正則表達式 |
| split\(str\) | 根據參數 str 做分割，將字串轉換為 Array 物件，舉例來說，假設變數 X 的值為 "JavaScript程式設計"，則 X.split\("a"\) 會傳回 \["J","v","Script程式設計"\] |
| slice\(i1,i2\) | 擷取片段，索引可為負 |
| substring\(i1,i2\) | 擷取片段，索引不可為負 |
| substr\(index,length\) | 從索引 index 擷取長度為 length 的子字串 |
| toUpperCase\(\) | 轉大寫 |
| toLowerCase\(\) | 轉小寫 |
| trim\(\) | 去除字首,字尾空白 |

### 轉為 HTML 標籤方法

| 名稱 | 說明 |
| :--- | :--- |
| anchor\(\) | 傳回 &lt;a&gt;str&lt;/a&gt; 標籤字串 |
| big\(\) | 傳回 &lt;big&gt;str&lt;/big&gt; 標籤字串 |
| blink\(\) | 傳回 &lt;blink&gt;str&lt;/blink&gt; 標籤字串 |
| bold\(\) | 傳回 &lt;b&gt;str&lt;/b&gt; 標籤字串 |
| fixed\(\) | 傳回 &lt;tt&gt;str&lt;/tt&gt; 標籤字串 |
| fontcolor\(color\) | 傳回 &lt;font color="color"&gt;str&lt;/font&gt; 標籤字串 |
| fontsize\(size\) | 傳回 &lt;font size="size"&gt;str&lt;/font&gt; 標籤字串 |
| italics\(\) | 傳回 &lt;i&gt;str&lt;/i&gt; 標籤字串 |
| link\(url\) | 傳回 &lt;a href="uri"&gt;str&lt;/a&gt; 標籤字串 |
| small\(\) | 傳回 &lt;small&gt;str&lt;/small&gt; 標籤字串 |
| strike\(\) | 傳回 &lt;strike&gt;str&lt;/strike&gt; 標籤字串 |
| sub\(\) | 傳回 &lt;sub&gt;str&lt;/sub&gt; 標籤字串 |
| sup\(\) | 傳回 &lt;sup&gt;str&lt;/sup&gt; 標籤字串 |

### 字串樣板 \(ES6\)

```javascript
let name = '小明'

傳統:
let str = '我叫做'+ name +'請多多指教'

樣板:
let str = `我叫做${name}請多多指教`

樣板也能插入 javascript 原始碼
例如 :
people = ['jack','kevin','mary']

let str = `
<ul>
    ${people.map(e=> `<li>我叫做 ${e}</li>` )}
</ul>
`
```

## 數字 (number)

### 基本

* 所有數值都以 IEEE 754 Double \(64 位元雙倍精確浮點數\) 表示
* 範圍  -2^1024 2^1024 = -10^307 ~ 10^307
* NaN 表示不當的數值運算
* Infinity 無限大
* -Infinity 負無限大

### 屬性

| 名稱 | 說明 |
| :--- | :--- |
| MAX\_VALUE | 傳回最大值，約 1.7976931348623157e+308 |
| MIN\_VALUE | 傳回最小值，約 5e-324 |
| NaN | 傳回 NaN |
| NEGATIVE\_INFINITY | 傳回 -infinity |
| POSITIVE\_INFINITY | 傳回 infinity |

### 方法

| 名稱 | 說明 |
| :--- | :--- |
| toString\(\) | 轉字串 |
| toFixed\(num\) | 將小數點後面的精確位數設定為參數 num 所指定的位數 |
| toPrecision\(num\) | 將精確位數設定為參數 num 所指定的位數 |
| toExponential\(\) | 轉科學表示法 |


## location 物件
網址資訊 URI

### 屬性

| 名稱 | 說明 |
| :--- | :--- |
| hash | URI 網址 \# 符號後面的資料 |
| host | URI 網址的主機名稱與通訊埠 |
| hostname | URI 網址的主機名稱 |
| href | URI 網址 |
| pathname | URI 網址的檔案名稱與路徑 |
| port | URI 網址的通訊埠 |
| protocol | URI 網址的通訊協定 |
| search | URI 網址 ? 符號後的資料 |

```text
例如目前網址: https://www.google.com/search?q=demo&oq=demo

hash: ""
host: "www.google.com"
hostname: "www.google.com"
href: "https://www.google.com/search?q=demo&oq=demo"
pathname: "/search"
port: ""
protocol: "https:"
search: "?q=demo&oq=demo"
```

### 方法

| 名稱 | 說明 |
| :--- | :--- |
| reload\(\) | 重新載入 |
| replace\(uri\) | 令瀏覽器載入並顯示參數 uri 所指定的網頁，取代目前開啟的網頁在瀏覽歷程記錄中的位置 |
| assign\(uri\) | 令瀏覽器載入並顯示參數 uri 所指定的網頁，相當於將 href 屬性設定為參數 uri |


## history 物件
瀏覽器的歷程記錄

### 屬性 \(唯讀\)

| 名稱 | 說明 |
| :--- | :--- |
| length | 紀錄筆數 |

### 方法

| 名稱 | 說明 |
| :--- | :--- |
| back\(\) | 上一頁 |
| forward\(\) | 下一頁 |
| go\(num\) | 上下幾頁 ，num 正負數來判斷 |


## navigator 物件
瀏覽器相關描述與系統資訊

###  屬性 \(唯讀\)

| 名稱 | 說明 |
| :--- | :--- |
| appCodeName | 瀏覽器的程式碼名稱 |
| appName | 瀏覽器的名稱 |
| appMinorVersion | 瀏覽器的次版本 |
| cpuClass | CPU 的類型 |
| cookieEnabled | 瀏覽器是否啟用 cookies 功能 |
| javaEnabled | 瀏覽器是否能夠執行 Java Applet |
| platform | 作業系統與硬體平台 |
| userProfile | 使用者 |
| systemLanguage | 系統預設語系 |
| userLanguage | 使用者設定的語系 |
| browserLanguage | 瀏覽器設定的語系 |
| appVersion | 瀏覽器的版本與作業系統的名稱 |
| userAgent | HTTP Request 中 user-agent 標頭的值 |
| onLine | 目前系統是否在線上 |
| geolocation | 瀏覽器目前的地理位置資訊 |


## screen 物件
螢幕資訊

### 屬性 \(唯讀\)

| 名稱 | 說明 |
| :--- | :--- |
| height | 高度，以像素為單位 |
| width | 寬度，以像素為單位 |
| availHeight | 螢幕可用高度，不包含工具列...等 |
| availWidth | 螢幕可用寬度，不包含工具列...等 |
| colorDepth | 螢幕的色彩深度 |


## window 物件
代表瀏覽器視窗，索引標籤或框架，JavaScript 物件均隸屬於 window 物件

### 屬性

| 名稱 | 說明 |
| :--- | :--- |
| closed | 視窗是否關閉 |
| defaultStatus | 視窗狀態列預設文字 |
| length | 視窗的框架數目 |
| name | 視窗名稱 |
| opener | 指向開啟視窗的呼叫者 |
| parent | 指向父框架 |
| self | 指向 window 物件本身 |
| status | 視窗的狀態列文字 |
| top | 指向頂層框架 |
| window | 指向 window 物件本身，和 self 屬性相同 |
| frames | 指向 window 物件本身，這是由框架所組成的陣列物件 |
| pageXOffset | 文件在視窗內向右捲動多少像素 |
| pageYOffset | 文件在視窗內向下捲動多少像素 |
| screenX | 視窗左上角在螢幕上的 X 軸座標 |
| screenY | 視窗左上角在螢幕上的 Y 軸座標 |
| innerHeight | 視窗內文件顯示區域的高度 \(以 px 為單位\) |
| innerWidth | 視窗內文件顯示區域的寬度 \(以 px 為單位\) |
| outerHeight | 視窗的總高度，包括工具列、捲軸、視窗邊框等 \(以 px 為單位\) |
| outerWidth | 視窗的總寬度，包括工具列、捲軸、視窗邊框等 \(以 px 為單位\) |

### 方法

| 名稱 | 說明 |
| :--- | :--- |
| alert\(msg\) | 彈出警告窗 |
| prompt\(msg,\[input\]\) | 彈出可輸入數值的對話框 |
| confirm\(msg\) | 彈出 \(確定/取消\) 視窗 |
| moveBy\(x,y\) | 移動視窗位置 偏移 \(x,y\) |
| moveTo\(x,y\) | 移動視窗至螢幕上 \(x,y\) |
| resizeBy\(x,y\) | 調整視框大小，寬度變化x，高度變化y |
| resizeTo\(x,y\) | 調正視窗至寬度 x，高度y |
| scrollBy\(x,y\) | 調整捲軸，X軸位移 x，Y軸位移 y |
| scrollTo\(x,y\) | 調整捲軸，令網頁內座標為\(x,y\)的位置顯示在左上角 |
| open\(uri,name,features\) | 開啟一個內容為 uri，名稱為 name，外觀為 features 的視窗，傳回值為新視窗的 window 物件 |
| close\(\) | 關閉視窗 |
| focus\(\) | 令視窗取得焦點 |
| print\(\) | 列印網頁 |
| setInterval\(exp,time\) | 啟動計時器，以參數 time\(毫秒\) 所指定的時間週期性執行參數 exp 所指定的運算式 |
| clearInterVal\(\) | 停止 setInterval\(\) 所啟動的計時器 |
| setTimeOut\(exp,time\) | 啟動計時器，以參數 time\(毫秒\) 所指定的時間到達時，執行參數 exp 所指定的運算式 |
| clearTimeOut\(\) | 停止 setTimeOut\(\) 所啟動的計時器 |

### open\(\) 方法的外觀參數

| 名稱 | 說明 |
| :--- | :--- |
| copyhistory=1 或 0 | 是否複製瀏覽歷程記錄 |
| directories=1 或 0 | 是否顯示導覽列 |
| fullscreen=1 或 0 | 是否全螢幕顯示 |
| location=1 或 0 | 是否顯示網址列 |
| menubar=1 或 0 | 是否顯示功能表列 |
| status=1 或 0 | 是否顯示狀態列 |
| toolbar=1 或 0 | 是否顯示工具列 |
| scrollbars=1 或 0 | 當文件內容超過視窗時，是否顯示捲軸 |
| resizeable=1 或 0 | 是否可以改變視窗大小 |
| height=n | 視窗的高度，n 為像素 |
| width=n | 視窗的寬地，n 為像素 |


## document 物件
window 的子物件，document 代表 HTML 文件本身

### 文件物件模型 \(DOM\)

DOM \(Document Object Model\)，這個架構主要是用來表示與操作 HTML 文件，當瀏覽器解析 HTML 文件時，會建立一個由多個物件所構成的集合，稱為 DOM tree，每個物件代表 HTML 文件的元素，而且每個物件有各自的屬性，方法及事件，能夠使用 JavaScript 操控，以營造動態網頁效果

### 常用屬性

| 名稱 | 說明 |
| :--- | :--- |
| charset | HTML 文件的字元編碼方式 |
| characterSet | HTML 文件的字元編碼方式，唯讀 |
| cookie | HTML 文件專屬的 cookie |
| defaultCharset | 瀏覽器預設的字元編碼方式，唯讀 |
| domain | 文件來源伺服器的網域名稱 |
| lastModified | HTML 文件最後一次修改的日期時間 |
| referer | 連結至此 HTML 文件的網址 |
| url | HTML 文件的網址 |
| title | HTML 文件中 &lt;title&gt; 元素的文字 |

### 常用方法

| 名稱 | 說明 |
| :--- | :--- |
| open\(type\) | 根據 type 的 MIME 類型開啟新的文件，若 type 省略或為 "text/html" 表示開啟新的 HTML 文件 |
| close\(\) | 關閉以 open\(\) 方法開啟的文件資料流，使緩衝區的輸出顯示在瀏覽器 |
| querySelector\(selectors\) | 取得 HTML 文件中 CSS 選擇器的元素 |
| querySelectorAll\(selectors\) | 取得 HTML 文件中 CSS 選擇器的所有元素，返回 NodeList 陣列 |
| getElementById\(id\) | 取得 HTML 文件中 id 屬性為 id 的元素 |
| getElementsByName\(name\) | 取得 HTML 文件中 name 屬性為 name 的元素 |
| getElementsByClassName\(name\) | 取得 HTML 文件中 class 屬性為 name 的元素 |
| getElementsByTagName\(name\) | 取得 HTML 文件中標籤名稱為 name 的元素 |
| write\(data\) | 將參數 data 所指定的字串輸出至瀏覽器 |
| writeln\(data\) | 將參數 data 所指定的字串或換行輸出至瀏覽器 |
| createComment\(data\) | 根據參數 data 所指定的字串建立並回傳一個新的 Comment 節點 |
| createElement\(name\) | 根據參數 name 所指定的元素名稱建立並傳回一個新的，空的 Element 節點 |
| createText\(data\) | 根據參數 data 所指定的字串建立並傳回一個新的 Text 節點 |
| execCommand\(cmd,flag,value\) | 可通過運行命令，以滑鼠來做一些操控，第一個參數代表命令，第二個參數代表是否要顯示特定用戶介面，預設為false，第三個參數代表命令所使用的參數值 |

### querySelector 與 getXXX 區別

```javascript
/* 假設有 ul 包含3個 li */

// querySelector 是靜態
var ul=document.querySelector('ul');
var list=ul.querySelectorAll('li');
for(var i=0;i<3;i++){
    ul.appendChild(document.createElement('li'));
}
//    這時創建了3個新 li,添加在 ul 中
console.log(list.length)
//    输出3,輸出的是新增前數量,而非總數量


// getElement 是動態
var ul=document.getElementsByTagName('ul')[0];
var list=ul.getElementsByTagName('li');
for(var i=0;i<3;i++){
    ul.appendChild(document.createElement('li'));
}
//    這時創建了3個新 li,添加在 ul 中
console.log(list.length)
//    输出6
```

### 集合

| 名稱 | 說明 |
| :--- | :--- |
| all | HTML 文件中所有物件 |
| anchors | HTML 文件中具備 name 屬性的 &lt;a&gt;  |
| links | HTML 文件中具備 href 屬性的 &lt;a&gt; 與 &lt;area&gt;  |
| forms | HTML 文件中的表單 |
| frames | HTML 文件中的框架 |
| images | HTML 文件中的圖片 |
| styleSheets | HTML 文件中使用 &lt;link&gt; 與 &lt;style&gt; 嵌入的樣式表 |
| embeds | HTML 文件中使用 &lt;embed&gt; 嵌入的資源 |
| applets | HTML 文件中的 Java Applets |
| plugins | HTML 文件中的外掛程式 |

#### 範例

```markup
<form name="myForm1">
    <input type="button" id="B1" value="按鈕1">
    <input type="button" id="B2" value="按鈕2">
</form>

<form name="myForm2">
    <input type="button" id="B3" value="按鈕3">
    <input type="button" id="B4" value="按鈕4">
</form>
```

```javascript
document.forms[0].B1.value // B1 value
document.forms[1].B3.value // B3 value
//or
document.forms.myForm1.B1.value
document.forms.myForm2.B3.value

```

### 子物件 body

document 只有一個子物件 body，代表 HTML 文件的主體，其屬性如下表

| 名稱 | 說明 |
| :--- | :--- |
| link | 表示尚未瀏覽的超連結文字色彩 |
| alink | 表示被選取的超連結文字色彩 |
| vlink | 表示已經被瀏覽的超連結文字色彩 |
| background | 表示背景圖片的相對或絕對位置 |
| bgColor | 網頁的背景色彩 |
| text | 網頁的文字色彩 |

#### 範例

```javascript
document.body.bgColor = "yellow";
```

### execCommand 命令

| 名稱 | 說明 |
| :--- | :--- |
| backColor | 改變文檔的背景顏色，在 styleWithCss 模式中，它會改變包含塊的背景顏色。這個指令要求傳入  字符串作為參數值傳入，注意在IE中這個指令用來設置文本背景顏色 |
| bold | 使文檔內容只讀或者可編輯此指令要求傳入一個布爾值true或者false \(不支持IE\) |
| copy | 將當前選定內容複製到剪貼板，不同瀏覽器使這種行為啟用的條件不同，並且隨著時間的推移而變化，檢查兼容性表，以確定是否可以在您的情況下使用它 |
| createLink | 只有當你有選區的時候才會從選區創建一個錨點鏈接，此指令要求一個HURF URI字符串作為參數傳入，這個 URI 必須至少包含一個字符，即使是個空格 \(IE瀏覽器將會船艦一個URI值為null的鏈接\) |
| cut | 剪切當前選擇內容複製到剪切板，不同瀏覽器使這種行為啟用的條件不同，並且隨著時間的推移而變化，檢查兼容性表，以確定是否可以在您的情況下使用它 |
| decreaseFontSize | 在選擇點或者插入點添加 標籤 \(IE不支持\) |
| delete | 刪除當前選擇內容 |
| enableInlineTableEditing | 啟用或禁用表格行和列的插入和刪除控件 \(IE不支持\) |
| enableObjectResizing | 啟用或禁用圖像和其他可調整對象的調整大小操作 \(IE不支持\) |
| fontName | 改變選擇點和插入點的文字名稱，此指令需要傳入一個文字名稱的字符串\(例如"Arial"\) 作為參數 |
| fontSize | 改變選擇點和插入點的文字大小，此指令需要一個 HTML font size \(1-7\) 作為參數傳入 |
| foreColor | 修改選擇點或插入點的文字顏色，此指令要求一個顏色值作為參數傳入 |
| formatBlock | 在包含當前選擇內容的行添加一個 HTML 塊元素標籤，如果已存在一個包含的塊元素則會替換 。此指令要求一個標籤的字符串作為一個參數傳入，幾乎所有的塊央視標籤都可以使用  \(IE只支持標題標籤 H1 - H6, ADDRESS, and PRE，這些標籤必須包含在分隔符&lt; &gt;中\) |
| forwardDelete | 此指令可以在光標位置之前刪除字符，它與點擊刪除鍵相同。 |
| heading | 在選擇點或者插入點增加一個標題標籤，需要傳入標籤名為參數  \(Internet Explorer 和 Safari不支持\) |
| hiliteColor | 改變選區或者插入點的背景顏色，需要傳入一個顏色值作為參數，要發揮作用必須啟用 \(IE不支持\) |
| increaseFontSize | 在選區或者插入點增加BIG標籤 \(IE不支持\) |
| indent | 縮進包含選區或插入點的行，在火狐中，如果選區包含多個行不同層次的縮進，則選區中最少縮進的行才會被縮進 |
| insertBrOnReturn | 控制是否用回車鍵插入一個 br 標籤或是講當前塊元素拆分成兩個。\(IE不支持\) |
| insertHorizontalRule | 在插入點插入水平線 \(刪除選區\) |
| insertHTML | 在插入點插入HTML\(刪除選區\) 需要傳入一個有效的HTML字符串作為參數 \(IE不支持\) |
| insertImage | 在插入點插入一張圖片 \(刪除選區\)。需要傳入圖片地址作為參數。此地址必須至少包含一個字符，哪怕是一個空格。\(IE將會創建一個空值的鏈接\) |
| insertOrderedList | 為選擇或插入點創建一個編號的有序列表 |
| insertUnorderedList | 為選擇或插入點創建一個未顯示的無序列表 |
| insertParagraph | 在選擇或當前行周圍插入段落 \(IE在插入點插入段落，並刪除選擇區內容\) |
| insertText | 在插入點插入給定的純文本\(刪除選擇區內容\) |
| italic | 為選擇或插入點打開/關閉斜體。 \(IE使用EM標記代替I\) |
| justifyCenter | 使選區或插入點居中 |
| justifyFull | 使選區或插入點每行排齊 |
| justifyLeft | 使選區或插入點左對齊 |
| justifyRight | 使選區或插入點右對齊 |
| outdent | 使包含選區或插入點的行伸出排 |
| paste | 在插入點粘貼剪貼板內容\(替換當前選擇區內容\) |
| redo | 重做以前的撤消命令 |
| removeFormat | 從當前選區中移除所有格式 |
| selectAll | 全選可編輯區域內容 |
| strikeThrough | 在選擇點或插入點切換打開或關閉 |
| subscript | 在選擇點或插入點切換下標打開或關閉 |
| superscript | 在選擇點或插入點切換上標打開或關閉 |
| underline | 切換選擇或插入點的下劃線打開或關閉 |
| undo | 撤消最後執行的命令 |
| unlink | 從選定的錨點鏈接中移除錨點標記 |
| styleWithCSS | 替換 useCSS 指令；參數使用如下：true用來在標記中修改/生成樣式屬性，false用來生成格式化元素 |


## element 物件
凡是用 getElementById() 等其他方式取得的 HTML 都是 element 物件

### 常用屬性

| 名稱 | 說明 |
| :--- | :--- |
| attributes | 返回元素的屬性的NamedNodeMap |
| childNodes | 返回元素的子節點的NodeList |
| firstChild | 返回元素的首個子節點 |
| lastChild | 返回元素的最後一個子節點 |
| nextSibling | 返回元素之後緊跟的節點 |
| nodeName | 返回節點的名稱 |
| nodeType | 返回元素的型別 |
| ownerDocument | 返回元素所屬的根元素\(document物件\) |
| parentNode | 返回元素的父節點 |
| previousSibling | 返回元素之前緊跟的節點 |
| tagName | 返回元素的名稱 |

### 常用方法

| 名稱 | 方法 |
| :--- | :--- |
| appendChild\(node\) | 向節點的子節點列表末尾新增新的子節點 |
| cloneNode\(true\) | 克隆節點 |
| getAttribute\(att\_name\) | 返回屬性的值 |
| getAttributeNode\(att\_name\) | 以 Attribute 物件返回屬性節點 |
| getElementsByTagName\(node\_name\) | 找到具有指定標籤名的子孫元素 |
| hasAttribute\(att\_name\) | 返回元素是否擁有指定的屬性 |
| hasAttributes\(\) | 返回元素是否擁有屬性 |
| hasChildNodes\(\) | 返回元素是否擁有子節點 |
| insertBefore\(new\_node,existing\_node\) | 在已有的子節點之前插入一新的子節點 |
| removeAttribute\(att\_name\) | 刪除指定的屬性 |
| removeAttributeNode\(att\_node\) | 刪除指定的屬性節點 |
| removeChild\(node\) | 刪除子節點 |
| replaceChild\(new\_node,old\_node\) | 替換子節點 |
| setAttribute\(name,value\) | 新增新的屬性或者改變屬性的值 |
| setAttribute\(att\_node\) | 新增新的屬性 |

