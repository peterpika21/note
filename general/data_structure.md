###### tags: `通用技術`

# 資料結構與演算法

## 時間複雜度

description: 時間複雜度是用來評斷演算法執行快慢的指標，而實務上我們通常用大 O 符號（Big O notation）

> 通常，我們會希望一個演算法至少能比 O\(n²\) 還要快，如果能到 O\(n\)、O\(1\) 甚至是 O\(log n\) 的話是最理想的情況

![](https://i.imgur.com/TZxxT3g.png)

:::info
演算法有多快不是以秒衡量，而是以步驟次數
:::

ex

```javascript
// 第一段時間複雜度為 n
for(let i = 0;i<n;i++){} 

// 第二段時間複雜度為 n^2
for(let i = 0;i<n;i++){ 
    for(let j = 0;i<n;i++){}
}

// 第三段時間複雜度為 n^2
for(let i = 0;i<n;i++){ 
    for(let j = 0;i<n;i++){}
}

整體時間複雜度為  2n² + n
我們可以把它歸類為  O(n²)
```

## 資料結構

description: 資料儲存在電腦記憶體，決定資料順序和位置的，就是資料結構

### 串列 / 列表 \(List\)

1. 列表結構的資料會排成一條線，便於追加或刪除，缺點是存取資料較費時
2. 每一個資料和一個指標 \(Pointer\) 配對，指向下一筆資料在記憶體中的位址

![](https://i.imgur.com/wE69boL.png)

### 陣列 \(Array\)

1. 陣列結構由相同類型的元素集合組成，相較於 List 更便於進行資料的存取，但追加 / 刪除資料卻比 List 費力
2. 因為儲存為連續的，所以能透過索引來計算記憶體位址，進而直接存取每個資料

### 堆疊 \(Stack\)

1. 結構類似由上而下堆放的概念，若要針對資料操作只能從資料最新的那一端開始，只允許再一端進行操作
2. 追加資料稱為 **push**
3. 輸出資料稱為 **pop**
4. 原理為**後進先出** \(Last In First Out\)，縮寫 **LIFO**

![](https://i.imgur.com/gk16qGK.png)

### 佇列 \(Queue\)

1. 結構類似堆疊，差異在於佇列可以在兩端進行資料的操作
2. 佇列只允許在後端 \(稱為 rear \) 追加資料至佇列中，這種操作稱為**進入佇列 \(Enqueue\)** 
3. 佇列只允許在前端 \(稱為 front \) 從佇列中輸出資料，這種操作稱為**輸出佇列 \(Dequeue\)**
4. 佇列原理為**先進先出** \(First In First Out\)，縮寫 **FIFO**

![](https://i.imgur.com/vWe3XD5.png)

### 雜湊表 \(Hash Table\)

1. 雜湊表的資料結構，是由**雜湊函數** \(Hash Function\) 有效率地進行數據搜尋
2. 儲存的資料為成對的 Key 與 Value
3. 使用雜湊函數計算出資料的 Key 所對應的雜湊值，並透過餘數運算取得餘數值，並將該數值作為索引位址，最後將資料存入該位置中
4. 當兩筆或兩筆以上不同的資料，經過雜湊函數或餘數運算後得到相同的值，也就是資料的儲存位置重複時，稱為**碰撞** \(Collision\)
5. 發生碰撞時，可以藉由列表將資料接續再先前已存入的資料之後，這種方法稱為**額外鏈結法** \(Separate Chaining\)
6. 發生碰撞時，最廣為應用的為**開放定址法** \(Open Addressing\)，發生碰撞會求出第二候補位址並儲存，若位址已滿則會繼續找下一個候補位址，直至找到空的位址
7. 雜湊表主要可分為兩大類：**額外鏈結法** \(Separate Chaining\) 以及**開放定址法** \(Open Addressing\)
8. 其中**開放定址法**根據資料存放的邏輯又分成兩種方法：**線性探測法** \(Linear Probing\) 以及**二次探查** \(Quadratic Probing\)
9. 雜湊表能夠彈性儲存數據，又可以快速讀取數據，常用於程式語言的關聯陣列 \(Associative Array\)

### 堆積 \(Heap\)

1. 資料結構為樹狀，用於實踐**優先佇列** \(Priority Queue\)
2. 優先佇列是資料結構的一種，每個元素都有各自的優先級，優先級最高的元素最先得到服務
3. 用來表示堆積的樹狀結構中，各個點稱為**節點** \(Node\)，資料被儲存於各個節點中
4. 堆積的每個節點最多可以擁有兩個**子節點** \(Child Node\)
5. 再堆積中儲存資料的規則：父節點必須大於子節點，且追加資料時，會從最下層的節點開始加入資料，當最下層被填滿時，往下產生新的一階，再從左方開始追加資料

![](https://i.imgur.com/Jl6U5Qa.png)

### 二元搜尋樹 \(Binary Search Tree\)

1. 資料結構為樹狀
2. 若任意節點的左子樹不空，則所有節點上的資料都會大於連結在其左邊節點上的資料，左子樹上的所有節點的值均小於它的根結點的值，所以最左側的節點即為最小資料
3. 若任意節點的右子樹不空，則所有節點上的數據都會小於連結在其右邊節點上的資料，右子樹上的所有節點的值均大於它的根結點的值，所以最右側的節點即為最大資料

![](https://i.imgur.com/6qCcH1d.png)

