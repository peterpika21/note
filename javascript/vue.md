###### tags: `Javascript`

# Vue

## vue實例

每個 vue 應用都是通過 vue 函數創建一個新的 vue 實例開始的

```javascript=
var vm = new Vue({
  
})
```

雖然沒有完全遵循 MVVM 模型，但是 Vue 的設計也受到了它的啟發。因此在文檔中經常會使用 vm \(ViewModel 的縮寫\) 這個變量名表示 Vue 實例。

除了數據屬性，Vue 實例還暴露了一些有用的實例屬性與方法。它們都有前綴 \$，以便與用戶定義的屬性區分開來。例如：

```javascript=
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

## 生命週期

![](https://i.imgur.com/AoQieYu.png)


## 鉤子函數使用時機

1. beforeCreate：
在實例初始化之後，**數據觀測(data observer)**和**event/watcher事件配置**之前被調用，注意是**之前**，此時data、watcher、methods統統沒有。 
這個時候的vue實例還什麼都沒有，但是$route是存在的，可以根據路由信息進行重定向之類的操作



2. created：
在實例已經創建完成之後被調用。在這一步，實例已完成以下配置：數據觀測(data observer)，屬性和方法的運算，watch/event事件回調。然而，掛載階段還沒開始，$el屬性目前不可見。
此時this.\$data可以訪問，watcher、events、methods也出現了，若根據後台接口動態改變data和methods的場景下，可以使用



3. beforeMount：
在掛載開始之前被調用，相關的**render函數**首次被調用。但是render正在執行中，此時DOM還是無法操作的。我打印了此時的vue實例對象，相比於created生命週期，此時只是多了一個\$el的屬性，然而其值為undefined。



4. mounted：
在掛載之後被調用。在這一步創建vm.\$el並替換el，並掛載到實例上。
此時元素已經渲染完成了，依賴於DOM的代碼就放在這裡吧~比如監聽DOM事件。



5. beforeUpdate：
\$vm.data更新之後，虛擬DOM重新渲染和打補丁之前被調用。
你可以在這個鉤子中進一步地修改\$vm.data，這不會觸發附加的重渲染過程。



6. updated：
虛擬DOM重新渲染和打補丁之後被調用。 
當這個鉤子被調用時，組件DOM的data已經更新，所以你現在可以執行依賴於DOM的操作。但是不要在此時修改data，否則會繼續觸發beforeUpdate、updated這兩個生命週期，進入死循環！



7. beforeDestroy：
實例被銷毀之前調用。在這一步，實例仍然完全可用。



8. destroyed：
Vue實例銷毀後調用。此時，Vue實例指示的所有東西已經解綁定，所有的事件監聽器都已經被移除，所有的子實例也已經被銷毀。 這時候能做的事情已經不多了，只能加點兒提示toast之類的東西吧。

## 常用指令與修飾符

### 指令

```javascript=
// 綁定 html 屬性,其中 url 是動態
img(:src="url")

// 事件
@event="方法"

// for 建議一定要搭配 key 使用
v-for="(item,index) in array"

// if 與 show
v-if 和 v-show 差異在前著若 false, DOM並不會存在,後者是把 DOM 隱藏
v-if 下方可以直接接 v-else

// class綁定 可綁定多 class
:class="{'class名稱': true/false , ...}"
```

### v-model 修飾符

```javascript=
// 輸入完成,離開input時才更新綁定數據
v-model.lazy

// 自動移除多餘空白
v-model.trim

// 轉成 number
v-model.number
```

### 點擊與滾輪修飾符

```javascript=
// 阻止冒泡
@click.stop="" 

// 阻止冒泡的反向,從外層開始阻止
@click.capture="" 

// 阻止默認
@click.prevent="" 

// 觸發自己
@click.self="" 

// 觸發一次
@click.once="" 

// 自己寫的小組件,觸發要加上
@click.native="" 

// 左鍵點擊
@click.left="" 

// 右鍵點擊
@click.right="" 

// 中鍵點擊
@click.middle="" 

// 滾輪,相當於給onscroll一個lazy修飾符
@scroll.passive="onScroll"
```

### 鍵盤修飾符

```javascript=
// 普通
.enter 
.tab
.delete //(刪除與退格鍵)
.space
.esc
.up
.down
.left
.right

// 系統
.ctrl
.alt
.meta
.shift

可通過全局 config.keyCodes 自定義按鍵碼
如: Vue.config.keyCodes.f1 = 112

可使用組合按鍵
如: @keyup.ctrl.67=""
```

### .sync 修飾符

```javascript=
// 父組件
<comp :myMessage="bar" @update:myMessage="func"></comp>
//js
func(e){
 this.bar = e;
}

// 子組件
func2(){
  this.$emit('update:myMessage',params);
}

可簡化成

// 父组件
<comp :myMessage.sync="bar"></comp> 
// 子组件
this.$emit('update:myMessage',params);

注意!!
使用sync的時候，子組件傳遞的事件名必須為update:value，
其中value必須與子組件中props中聲明的名稱完全一致

注意帶有.sync 修飾符的v-bind 不能和表達式一起使用

將v-bind.sync 用在一個字面量的物件上，
例如v-bind.sync="{ title: doc.title }"，是無法正常工作的
```

## watch \(監聽\)

顧名思義是監聽響應式屬性,變化後做什麼事情

```javascript=
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
```

進階用法

```javascript=
watch: {
  question: {
    handler(new, old) {
      // 處理
    },
    // 代表在 watch 聲明 question 後,立即執行 handler
    immediate: true,
    // 深度監聽,修改物件或陣列時需要用深度監聽才有效
    deep: true
  }
}
```

物件監聽也能用雙引號包裹 不一定要 deep

```javascript=
watch: {
  'obj.data': {
    handler(new, old) {
      // 處理
    },
  }
}
```

監聽 \$refs 需使用 this.watch

```javascript=
// 需要監聽的選項
let watchData = [
  '$refs.xxx1',
  '$refs.xxx2',
  '$refs.xxx3',
  '$refs.xxx4'
]

watchData.forEach(e => {
  this.$watch(e,() => {
      // 處理
    },{ deep: true }
  )
})
```

## computed \(計算\)

:::info
不可在計算式內直接更改實例
:::


表達式雖然好用,但是太長得表達式反而難以維護,這時可以用計算

```javascript=
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

計算 VS 方法,兩者確實能達到相同效果,差異在於計算一定是某個響應式屬性變動後才會觸發

計算 VS 監聽 , 有時候計算更能取代監聽效果 , 使其不氾濫使用

```javascript=
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})

// 可以改成
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
```

計算默認只有 getter , 但我們也可以提供 setter

```javascript=
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

## key

Vue 會盡可能高效地渲染元素，通常會復用已有元素而不是從頭開始渲染。這麼做除了使 Vue 變得非常快之外，還有其它一些好處。例如，如果你允許用戶在不同的登錄方式之間切換：

```javascript=
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

```javascript=
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

若沒有 key 值, 切換時會保留數值 , 加上 key 代表這兩個元素是完全獨立的, 數值跟者被清除


## 自定義組件的 v-model

```htmlmixed=
<input v-model="parentData">

其實就是

<input :value="parentData"
       @input="parentData = $event.target.value">

```

綁定頁面

```javascript=
<template>
  <div>
    <myComponent v-model="parentData"></myComponent>
  </div>
</template>

<script>
import myComponent from "~/components/myComponent.vue";

export default {
  components: {
    myComponent
  },
  data() {
    return {
      parentData: "hello world"
    };
  }
};
</script>

```

組件

```javascript=
<template>
  <input type="text" v-model="childData" />
</template>
<script>

export default {
  props: ["value"],
  data() {
    return {
      childData: this.value
    };
  },
  watch: {
    childData(value) {
      this.$emit("input", value);
    },
    value(newVal) {
      this.childData = newVal
    }
  }
};
</script>

```

## 註冊組件

全局註冊

```javascript=
import Vue from 'vue'
import myConponent from '~/components/myConponent'

Vue.component('myConponent', myConponent)

```

局部註冊

```javascript=
import Vue from 'vue'
import myConponent from '~/components/myConponent'

export default {
  components: {
    myConponent
  }
}
```

## props 與 emit
組件資訊的傳入與傳出

props

```javascript=
// 父組件
<myConponent :data="XXX"></myConponent >

// 子組件
export default {
    props:{
        data:{
            type: Array, // 組件型態
            default: [], // 預設值
            required: true // 是否必填
    }
}

// type 的類型如下
String
Number
Boolean
Array
Object
Date
Function
Symbol

注意!! 不可直接更改 props的值, 若要修正,創建時必須賦予其他變數
```

emit

```javascript=
// 父組件
<myConponent @updata="updata"></myConponent >
export default {
    methods:{
        updata(value){
            // 接收子組件傳出的值
        }
    }
}


// 子組件
export default {
    methods:{
        setData(value){
            this.$emit('updata',value)
        }
    }
}

```

## slot 插槽
作用於組件上的自定義區塊

在2.6.0版後, slot 改用 v-slot 取代

範例模板

```javascript=
自訂模板
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>


使用
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

備註
v-slot:default 是默認插槽
v-slot 只能添加到 <template> 或自定義組件上，這點與棄用的 slot 屬性不同
```

也可以訪問子組件資訊

```javascript=
// current-user 組件內容
<span>
  <slot>{{ user.lastName }}</slot>
</span>


// 使用組件
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>


// 多個插槽情況
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```

## 動態組件 or 異步組件

動態組件範例如下

```javascript=
<component v-bind:is="currentTabComponent"></component>
```

在切換組件時，vue 都會重新創建組件，這是非常有用的，但有的時候我們不要組件重新被創建

這時就能使用 keep-alive

```javascript=
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

異步組件，在大型應用中，有些組件並不需要馬上載入就可以使用異步組件增加載入速度

```javascript=
常用寫法 我們可以註冊全域與區域的異步組件

// 全域
Vue.component(
  'async-webpack-example',
  // 
  () => import('./my-async-component')
)

// 區域
  components: {
    'my-component': () => import('./my-async-component')
  }
  
// 異步組件的工廠函式
const AsyncComponent = () => ({

  // 需要加載的組件 (应该是一个 `Promise` 对象)
  component: import('./MyComponent.vue'),
  
  // 加載時使用的組件
  loading: LoadingComponent,
  
  // 失敗時使用的組件
  error: ErrorComponent,
  
  // 延遲時間, 默認 200(毫秒)
  delay: 200,
  
  // 超過時間則加載失敗組件, 默認 Infinity
  timeout: 3000
})
```

## directive
除了原本 vue 提供的指令外，也能自創指令

### 鉤子函數

bind：只調用一次，指令第一次綁定到元素時調用。在這裡可以進行一次性的初始化設置。 

inserted：被綁定元素插入父節點時調用 \(僅保證父節點存在，但不一定已被插入文檔中\)。 

update：所在組件的 VNode 更新時調用，但是可能發生在其子 VNode 更新之前。指令的值可能發生了改變，也可能沒有。但是你可以通過比較更新前後的值來忽略不必要的模板更新 \(詳細的鉤子函數參數見下\)。 

componentUpdated：指令所在組件的 VNode 及其子 VNode 全部更新後調用。 

unbind：只調用一次，指令與元素解綁時調用。

### 鉤子函數參數

el：指令所綁定的元素，可以用來直接操作 DOM 。 

binding：一個對象，包含以下屬性： 

     name：指令名。

     value：指令的綁定值。

     oldValue：指令綁定的前一個值，僅在 update 和 componentUpdated 鉤子中可用。

     expression：字符串形式的指令表達式。

vnode：Vue 編譯生成的虛擬節點。

oldVnode：上一個虛擬節點，僅在 update 和 componentUpdated 鉤子中可用。



官方範例

```javascript=
// 全局註冊

Vue.directive('focus', {
  inserted: function (el) {
    el.focus()
  }
})
```

## filter
可使用過濾器格式化一些常見的文本

使用方式

```javascript=
// 雙括號中
<p> {{ message | capitalize }} </p>

//全局註冊
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})
```


## 過度 & 動畫

過度的類名

v-enter：定義進入過渡的開始狀態。在元素被插入之前生效，在元素被插入之後的下一幀移除。

v-enter-active：定義進入過渡生效時的狀態。在整個進入過渡的階段中應用，在元素被插入之前生效，在過渡/動畫完成之後移除。這個類可以被用來定義進入過渡的過程時間，延遲和曲線函數。

v-enter-to: 2.1.8版及以上 定義進入過渡的結束狀態。在元素被插入之後下一幀生效 \(與此同時 v-enter 被移除\)，在過渡/動畫完成之後移除。

v-leave: 定義離開過渡的開始狀態。在離開過渡被觸發時立刻生效，下一幀被移除。

v-leave-active：定義離開過渡生效時的狀態。在整個離開過渡的階段中應用，在離開過渡被觸發時立刻生效，在過渡/動畫完成之後移除。這個類可以被用來定義離開過渡的過程時間，延遲和曲線函數。

v-leave-to: 2.1.8版及以上 定義離開過渡的結束狀態。在離開過渡被觸發之後下一幀生效 \(與此同時 v-leave 被刪除\)，在過渡/動畫完成之後移除。



單元素 / 組件的過度 通常使用 css , 至於 JS 的動畫請看官方說明

```javascript=
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>

<style>

// 全局過度動畫效果
.page-enter-active,
.page-leave-active {
  transition: opacity .3s;
}

.page-enter,
.page-leave-active {
  opacity: 0;
}


// 自訂過度動畫 (自訂名稱 + 類名)

.fadeRight-enter-active {
  animation: fadeInRight 0.3s;
}

.fadeRight-leave-active {
  animation: fadeOutRight 0.3s;
}

@keyframes fadeInRight {
  from {
    opacity: 0;
    -webkit-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
  }

  to {
    opacity: 1;
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
  }
}

.fadeInRight {
  -webkit-animation-name: fadeInRight;
  animation-name: fadeInRight;
}
</style>
```

多個組件的過度 不須使用 key

```javascript=
<transition name="component-fade" mode="out-in">
  <component v-bind:is="view"></component>
</transition>

<style>
.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-to
/* .component-fade-leave-active for below version 2.1.8 */ {
  opacity: 0;
}
</style>
```

列表的過度 使用  `<transition-group>` 组件

```javascript=
<div id="list-demo" class="demo">
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>

<style>
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to
/* .list-leave-active for below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
```

狀態的過度，此範例使用 GreenSock.js

當 animatedNumber 值更新時，就會觸發動畫效果

```javascript=
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/1.20.3/TweenMax.min.js"></script>

<div id="animated-number-demo">
  <input v-model.number="number" type="number" step="20">
  <p>{{ animatedNumber }}</p>
</div>

new Vue({
  el: '#animated-number-demo',
  data: {
    number: 0,
    tweenedNumber: 0
  },
  computed: {
    animatedNumber: function() {
      return this.tweenedNumber.toFixed(0);
    }
  },
  watch: {
    number: function(newValue) {
      TweenLite.to(this.$data, 0.5, { tweenedNumber: newValue });
    }
  }
})
```

## $refs
可訪問當前組件資訊

```javascript=
例如自訂組件
<base-input ref="usernameInput"></base-input>

consloe.log(this.$refs) 能抓出組件內資訊, 也可調用方法

注意!!
$refs只會在組件渲染完成之後生效，並且它們不是響應式的。
你應該避免在模板或計算屬性中訪問$refs。
```


## vue.use

1. 這個方法接收一個引數 , 這個引數必須具有install方法 , Vue.use函式內部會呼叫引數的install方法

2.  註冊成功之後會給外掛新增一個installed的屬性值為true , vue 會自動檢測 , 避免重複

3.  外掛的install方法將接收兩個引數，第一個是引數是Vue，第二個引數是配置項options

```javascript=
import Vue from 'vue';

// 這個外掛必須具有install方法
const plugin = {
  install (Vue, options) {
  
    // 新增全域性方法或者屬性
    Vue.myGlobMethod = function () {};
    
    // 新增全域性指令
    Vue.directive();
    
    // 新增混入
    Vue.mixin();
    
    // 新增例項方法
    Vue.prototype.$xxx = function () {};
    
    // 註冊全域性元件
    Vue.component()
  }
}

// Vue.use內部會呼叫plugin的install方法
Vue.use(plugin);
```

## vuex
vue 的 component 若不相鄰時，傳遞資訊會變得很麻煩，這時可使用 vuex 管理共享狀態資訊，通常有一定規模的專案才比較會使用

### 概念圖

![](https://i.imgur.com/dyek4Z0.png)

### state

儲存共享資訊的地方，類似 vue 內的 data

:::info
不可直接改變 state value，必須經由 mutations
:::



### mutations

更改 state 狀態的地方

```javascript=
mutations: {
  // 習慣性大寫
  SET_DEMO(state, payload) {
  // state 就是 vuex state
  // payload 則是 傳進來的值
    state.company.logo = payload
  }
}
```

### actions

非同步資訊都放在這，但記住這邊不可直接更改狀態，必須 commit 到 mutations

```javascript=
actions: {
  // context 物件具有此 store 實例
  getAuth(context) {
    // 取得該用戶權限
    this.app.apolloProvider.defaultClient
      .query({
        query: demo
      })
      .then(res => {
        context.commit('SET_DEMO', res)
      })
  }
}
```

#### 使用 promise

```javascript=
// 假设 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

#### dispatch 與 commit 差異

```javascript=
// dispatch 用於非同步操作
this.$store.dispatch('方法名',值)

// commit 用於同步操作
this.$store.commit('方法名',值)
```

### getter

相較於 computed ，也可直接在 vuex 內做計算

```javascript=
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

### Module

vuex 允許切分模塊，方便管理

#### 主 module

```javascript=
import Vue from 'vue'
import Vuex from 'vuex'
import children from './children'

Vue.use(Vuex)

const store = () =>
  new Vuex.Store({
    modules: {
      children // 引入子模塊
    },
    strict: true
  })

export default store

```

#### 子 module

```javascript=
export default {
  // vuex 內容  
}

```

### 在組件中使用 map

如果資訊太多，為了整潔可以直接使用映射的方式把 vuex 資訊拉到各組件使用

```javascript=
import { mapState , mapMutations , mapActions , mapGetters  } from 'vuex'

export default {
  computed: {
    ...mapGetters(['demo',...]),
    ...mapState(['demo'],...)
  }
  methods: {
    ...mapMutations(['demo'],...),
    ...mapActions(['demo'],...)
  }
}
```


