###### tags: `Javascript`

# 設計模式(design pattern)

> 設計模式通常是被使用在軟體工程中的一個名詞，其代表著替軟體設計中常見發生的問題，提供一個可重複使用的解決方案

創建型
===

這種模式主要是為了處理物件的創建機制，包含以下幾種模式

## 建構子模式(Constructor Pattern)

建構子模式是Javascript裡，最普遍使用來創建給定新物件的一種設計模式

```javascript=
/* 傳統用法 */
function hero(name, content) {
  this.name = name;
  this.content = content;

  this.getDetial = () => {
    return `${this.name}說:${this.content}`;
  };
}

/* ES6 class用法 */
class hero {
  constructor(name, content) {
    this.name = name;
    this.content = content;
  }
  getDetial() {
    return `${this.name}說:${this.content}`;
  }
}

let user = new hero("小王", "哈囉");
console.log(user.getDetial());
```

## 工廠模式(Factory Pattern)

經常被使用在，我們需要同時管理與操作，多個有相似特性卻仍不同的物件集合

```javascript=
// 定義工廠類別
class BallFactory {
  create(type) {
    let ball;

    if (type === "football") {
      ball = new Football();
    }

    if (type === "basketball") {
      ball = new Basketball();
    }

    // 工廠內可加入通用函式或變數
    ball.roll = () => {
      return `這是${ball.type}通用函式`;
    };

    return ball;
  }
}

// 足球類別
class Football {
  constructor() {
    this.type = "足球";
  }
  play() {
    return `來玩${this.type}`;
  }
}

// 籃球類別
class Basketball {
  constructor() {
    this.type = "籃球";
  }
  play() {
    return `來玩${this.type}`;
  }
}

const factory = new BallFactory();
const myFootball = factory.create("football");
const myBasketball = factory.create("basketball");
```

## 原型模式(Prototype Pattern)

這個設計模式特別重要且對於Javascript很有益處，因為Javascript使用原型繼承代替類別式物件導向

```javascript=
let car = {
  color: "紅色",
};

// 可加入新值或取代舊值
const BMW = Object.create(car, {
  name: { value: "BMW" },
  color: { value: "藍色" },
});

console.log(BMW.color); // 藍色
console.log(BMW.__proto__.color); // 紅色
```

## 單例模式(Singleton Pattern)

只能存在唯一實例，當不存在實例則創建，反之則回傳參考

```javascript=
class factory {
  constructor(data) {
    // 如果實例已存在則採用
    if (factory.exists) {
      return factory.instance;
    }
    this.data = data;
    factory.instance = this;
    factory.exists = true;
  }
}

const demo1 = new factory("123");
const demo2 = new factory("456");
console.log(demo1.data); // 123
console.log(demo2.data); // 123 會得到參考值
```

## 介面卡模式(Adapter Pattern)

提供一個介面(interface)將類別可轉換至另外一個，這個模式經常被使用來，同時包裝新的重構好的API，且讓舊的API依然可以使用

```javascript=
// old interface
class OldCalculator {
  constructor() {
    this.operations = function (term1, term2, operation) {
      switch (operation) {
        case "add":
          return term1 + term2;
        case "sub":
          return term1 - term2;
        default:
          return NaN;
      }
    };
  }
}

// new interface
class NewCalculator {
  constructor() {
    this.add = function (term1, term2) {
      return term1 + term2;
    };
    this.sub = function (term1, term2) {
      return term1 - term2;
    };
  }
}

// 由於新的API跟舊的方法不一致，這邊把新的API封裝成與舊的相符
class CalcAdapter {
  constructor() {
    const newCalc = new NewCalculator();
    this.operations = function (term1, term2, operation) {
      switch (operation) {
        case "add":
          return newCalc.add(term1, term2);
        case "sub":
          return newCalc.sub(term1, term2);
        default:
          return NaN;
      }
    };
  }
}

const oldCalc = new OldCalculator();
const newCalc = new NewCalculator();
const adaptedCalc = new CalcAdapter();
console.log(oldCalc.operations(10, 5, "add")); // 15
console.log(newCalc.add(10, 5)); // 15
console.log(adaptedCalc.operations(10, 5, "add")); // 15;
```

結構型模式
===

行為型模式
===

