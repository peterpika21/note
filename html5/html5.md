###### tags: `HTML`

# HTML5

## 基礎

```markup
// 定義 HTML
<!DOCTYPE html>
<html lang="en">

// 放網站資訊
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

// 放網站內容
<body>


</body>
</html>
```

> css 通常放 head 內 ， script 通常放 body 尾

## &lt;head&gt;

```markup
  <head>
    // 網站編碼
    <meta charset="UTF-8" />
    
    // 作者
    <meta name="author" content="ichien" />
    
    // 響應式網站必須加上
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    
    // IE 兼容模式
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    
    // 網站標題
    <title>Document</title>
  </head>
```

> name 資訊 可優化 SEO ， content 即為 metadata 的內容

| name值 | 說明 |
| :--- | :--- |
| application-name | 應用程式名稱 |
| author | 作者名稱 |
| generator | 編輯程式 |
| keywords | 關聯的關鍵字 |
| description | 描述文字 |


> http-equiv 可以取代 name 屬性，因為 HTTP 伺服器是使用 http-equiv 屬性蒐集 HTTP 標頭


### &lt;link&gt; 指令文件的關聯 , 放在 &lt;head&gt; 內

| 關聯 | 說明 |
| :--- | :--- |
| appendix | 附錄 |
| alternate | 替代表示方式 |
| author | 作者 |
| contents | 內容 |
| index | 索引 |
| glossary | 名詞解釋 |
| copyright | 版權宣告 |
| next | 下一頁 \(和 rel 一起使用\) |
| pre | 上一頁 \(和 rev 一起使用\) |
| start | 第一個文件 |
| help | 線上說明 |
| bookmark | 書籤 |
| stylesheet | css 樣式表 |
| search | 搜尋資源 |
| top | 首頁 |

### 屬性

| 屬性 | 說明 |
| :--- | :--- |
| charset | 編碼 |
| href | 文件的相對或絕對位置 |
| hreflang | href 語系 |
| madia | 設備 |
| name | 指定名稱 |
| rel | 指定目前文件與其他文件的關聯 |
| rev | 指定目前文件與其他文件的反向關聯 |
| target | 指定目標框架的名稱 |
| type | 指定內容類型 |

#### 例子

```markup
  <head>
    <link type="text/html" rel="help" href="help.html">
    <link rel="top" href="http://www.xxx.com/">
    <link rev="pre" href="backpage.html">
    <link type="text/css" rel="stylesheet" href="h1.css">
  </head>
```

#### 網頁自動導向

```markup
  <head>
    // 5秒後自動導向 http://www.xxx.com
    <meta http-equiv="refresh" content="5;url="http://www.xxx.com">
  </head>
```

## 全域屬性

| 屬性 | 說明 |
| :--- | :--- |
| title | 元素標題，可能用於提示文字 |
| id | 識別字 \(具唯一性\) |
| class | 類別 |
| style | 套用 css stye |
| dir | 文字方向，ltr\(左至右\)， rtl\(右至左\) |
| lang | 元素語系 |
| accesskey="key" | alt + key值，聚焦元素 |
| tabindex="n" | 按 \[Tab\] 時在元素間切換聚焦，n 越小順序越高 |
| draggable | 是否能被拖曳 |
| hidden | 隱藏元素 |
| data-\*="..." | 透過自定義屬性將資訊傳給 script |

## 事件屬性

| 屬性 | 說明 |
| :--- | :--- |
| onload | 瀏覽器載入後觸發，通常寫在 &lt;body&gt;，也可寫在 js 內 \(window.onload\) |
| onunload | 離開瀏覽器觸發，某些瀏覽器無作用 |
| onclick | 點擊觸發 |
| ondblclick | 雙擊觸發 |
| onmousedown | 按下滑鼠按鍵觸發 |
| onmouseup | 彈起滑鼠按鍵觸發 |
| onmouseover | 鼠標移過觸發 |
| onmousemove | 鼠標在元素上移動觸發 |
| onmouseout | 鼠標離開元素觸發 |
| onfocus | 焦點觸發 |
| onblur | 模糊觸發 |
| onkeypress | 按鍵點擊觸發 |
| onkeydown | 按鍵按下觸發 |
| onkeyup | 按鍵彈起觸發 |
| onsubmit | 傳送表單觸發 |
| onreset | 清除表單觸發 |
| onselect | 選擇觸發 |
| onchange | 修改表單欄位觸發 |

## 文字格式

| 標籤 | 說明 |
| :--- | :--- |
| &lt;b&gt; | 粗體 |
| &lt;i&gt; | 斜體 |
| &lt;u&gt; | 底線 |
| &lt;s&gt; | 刪除線 |
| &lt;mark&gt; | 螢光標記 |
| &lt;sub&gt; | 下標 |
| &lt;sup&gt; | 上標 |
| &lt;small&gt; | 小字型 |
| &lt;em&gt; | 強調斜體 |
| &lt;strong&gt; | 強調粗體 |
| &lt;dfn&gt; | 定義 |
| &lt;code&gt; | 程式碼 |
| &lt;samp&gt; | 範例文字 |
| &lt;kdd&gt; | 鍵盤文字 |
| &lt;var&gt; | 變數文字 |
| &lt;cite&gt; | 引用文字 |
| &lt;abbr&gt; | 縮寫文字 |
| &lt;q&gt; | 引用語 |



## 常見標籤

| 區塊 | 說明 | 注意 |
| :--- | :--- | :--- |
| &lt;h1&gt; - &lt;h6&gt; | 標題 | 網站通常只有一個 h1 |
| &lt;p&gt; | 段落 |  |
| &lt;span&gt; | 行內文字 |  |
| &lt;div&gt; | 區塊 |  |
| &lt;hr&gt; | 水平線 |  |
| &lt;br&gt; | 換行 |  |
| &lt;ul&gt;,&lt;ol&gt;,&lt;li&gt; | 項目清單 |  |
| &lt;dl&gt;,&lt;dt&gt;,&lt;dd&gt; | 定義清單 |  |
| &lt;header&gt; | 頁首 |  |
| &lt;footer&gt; | 頁尾 |  |
| &lt;nav&gt; | 導航列 |  |
| &lt;aside&gt; | 側邊攔 |  |
| &lt;section&gt; | 通用區段 |  |
| &lt;article&gt; | 本文或獨立內容 |  |
| &lt;address&gt; | 聯絡資訊 |  |
| &lt;hgroup&gt; | 群組標題 |  |
| &lt;pre&gt; | 內容格式化 |  |
| &lt;marquee&gt; | 跑馬燈 |  |





## 表單

### &lt;form&gt; 表單

| 屬性 | 說明 |
| :--- | :--- |
| accept-charset | 編碼 |
| action | 處理表單的 uri |
| accept | 指定 MIME 類型 |
| enctype | 指定傳回 web 的編碼方式 , 表單資料傳到電子郵件要用 enctype="text/plain" |
| method | 處理方式 \(get , post\) |
| name | 表單名稱 |
| targer | 目標框架 |
| autocomplete | 是否啟用自動完成功能 |
| novalidate | 指定在提交表單時不要進行驗證 |

### &lt;input&gt; 輸入框

| 屬性 | 說明 |
| :--- | :--- |
| accept | 提交的 MIME 類型 |
| checked | 選擇扭設為以選取狀態 |
| disabled | 禁用 |
| maxlength | 最多字元數 |
| name | 名稱 \(英文且唯一\) |
| notab | 不允許用 tab 移至欄位 |
| readonly | 唯讀 |
| size | 字元數 , 與 maxlength 差別在於 size 是畫面上可以看到的字元數 |
| src | type="image" 時的提交按鈕位置 |
| type | 類型 |
| usemap | 瀏覽器端影像地圖所在的檔案位置及名稱 |
| value | 初始值 |
| form="xxx" | 指定表單欄為隸屬於 ID 為 xxx 的表單 |
| min | 最小值 |
| max | 最大值 |
| step | 間格 |
| required | 必填 |
| multiple | 提交多個 |
| pattern | 指定輸入格式 如 pattern="\[0-9\]{4}\(\-\[0-9\]{6}" |
| autocomplete | 自動完成 |
| autofocus | 自動聚焦 |
| placeholder | 提示文字 |

#### type 類型

| type="屬性" | 說明 |
| :--- | :--- |
| text | 文字 |
| password | 密碼欄位 |
| radio | 單選扭 |
| checkbox | 多選扭 |
| submit | 提交扭 |
| reset | 重新輸入 |
| file | 上傳檔案 |
| image | 圖片提交 |
| button | 按鈕 |
| hidden | 隱藏欄位 |
| email | 電郵 |
| url | 網址 |
| search | 搜尋 |
| tel | 電話 |
| number | 數字 |
| range | 範圍數字 |
| color | 色彩 |
| date | 日期 |
| time | 時間 |
| datetime | 世界標準時間 |
| month | 月份 |
| week | 周 |
| datetime-local | 本地日期時間 |

### &lt;textarea&gt; 多行

| 屬性 | 說明 |
| :--- | :--- |
| cols | 每行字數 |
| rows | 行數 |
| disabled | 禁用 |
| name | 名稱 |
| readonly | 唯讀 |
| form="xxx" | 屬於 ID xxx 的表單 |
| required | 必填 |
| autofocus | 自動聚焦 |
| placeholder | 提示文字 |

### &lt;select&gt; 下拉選單

| 屬性 | 說明 |
| :--- | :--- |
| multiple | 多選 |
| name | 名稱 |
| readonly | 唯獨 |
| size | 高度 |
| form="xxx" | 屬於 ID xxx 的表單 |
| required | 必填 |
| autofocus | 自動聚焦 |

#### &lt;optgroup label="xxx"&gt; 於 select 內的群組分類

#### &lt;option&gt; 於 select 內的選單

| 屬性 | 說明 |
| :--- | :--- |
| disabled | 禁用 |
| selected | 預先選取項目 |
| value | 下拉選單的值 |

### &lt;label&gt; 標籤文字

| 屬姓 | 說明 |
| :--- | :--- |
| for="xxx" | 與 ID 是 xxx 的表單產生關聯 |
| form="xxx" | 指定隸屬於 ID 為 xxx 的表單 |

#### ex

```markup
<form>
// 點選 label 區塊能對應相同 id 的 input
<label for="male">Male</label>
<input type="radio" name="sex" id="male" />
</form>
```



## 表格

### &lt;table&gt; 表格

| 屬性 | 說明 |
| :--- | :--- |
| summary | 說明文字 |
| width | 寬度 |
| border | 框線大小 |
| cellpadding | 指定儲存格墊充 |
| cellspacing | 指定儲存格間距 |
| frame | 外框線顯示方式 |
| rules | 內框線顯示方式 |

### &lt;tr&gt; 行

| 屬性 | 說明 |
| :--- | :--- |
| align | 水平對齊 |
| valign | 垂直對齊 |
| char | 指定某儲存格要對其的字元 \(當 align = "char" 時\) |
| charoff | 指定某列儲存格要對齊的字元是從左邊數來第幾個 |

### &lt;th&gt; 標題儲存格

### &lt;td&gt; 行內儲存格

| 屬性 | 說明 |
| :--- | :--- |
| aling | 水平對齊 |
| valign | 垂直對齊 |
| char | 指定某儲存格要對其的字元 \(當 align = "char" 時\) |
| charoff | 指定某列儲存格要對齊的字元是從左邊數來第幾個 |
| abbr | 根據儲存格的內容指定一個縮寫 |
| axis | 根據儲存格的內容指定一個分類 |
| bordercolor | 框線色彩 |
| colspan | 指定某個儲存格是由幾欄合併而成 |
| rowspan | 指定某個儲存格是由幾列合併而成 |
| headers | 指定提供標頭資訊的儲存格 |
| scope | 指定目前的標頭儲存格是提供了哪些標頭資訊 |

### &lt;thead&gt; , &lt;tbody&gt; , &lt;tfoot&gt;  適當用表頭,體,尾區分 能方便閱讀

## 超連結與圖片

### &lt;a&gt; \(超連結\) , 也可當作茅點使用

| 屬性 | 說明 |
| :--- | :--- |
| href | 指定連結的相對或絕對位置 |
| target | 何處打開文檔  常用:\_blank \(開新窗\) |
| charset | 編碼 |
| name | 書籤名稱 |
| type | 內容類型 |
| media | 設備 預設為 all |
| hreflang | href 語系 |
| coords | 指定影像地圖的熱點座標 |
| shape | 指定影像地圖的熱點形狀 |
| rel | 指定從目前文件到 href 屬性指定文件的關聯 |
| rev | 指定從 href 屬性指定文件到目前文件的關聯 |

### &lt;img&gt; 圖片

| 屬性 | 說明 |
| :--- | :--- |
| src | 指定連結的相對或絕對位置 |
| name | 指定名稱 供 script 使用 |
| alt | 找不到圖片的替代文字 |
| longdesc | 說明文字 |
| width | 寬 |
| height | 高 |
| ismap | 指定為伺服器端影像地圖 |
| usemap | 指定影像地圖所在的檔案位置 |
| lowsrc | 指定低解析度圖片的位置 |
| crossorigin | 允許跨文件存取 |

## 影音多媒體

### &lt;video&gt; 影片

| 屬性 | 說明 |
| :--- | :--- |
| src | 來源 |
| poster | 播方前的畫面圖片 |
| perload | 是否在載入網頁時預先下載影片至緩衝區 |
| autoplay | 自動播放 |
| loop | 循環播放 |
| muted | 靜音 |
| controls | 指定要顯示瀏覽器內件的控制面板 |
| width | 寬 |
| height | 高 |

### &lt;audio&gt; 聲音 , 用法與 video 類似

### &lt;iframe&gt; 嵌入浮動框架

| 屬性 | 說明 |
| :--- | :--- |
| align | 對齊方式 |
| name | 名稱 \(英文且唯一\) |
| src | 來源 |
| srolling | 顯示卷軸 \(yes/no\) |
| width | 寬 |
| height | 高 |
| marginheight | 上下 margin |
| marginweight | 左右 margin |
| frameborder | 是否顯示浮動框線 \(1,0\) |
| seamless | 指定已無縫的方式顯示浮動框架內容 |
| sandbox | 指定一組限制套用到浮動框架內容 |
| srcdoc | 指定浮動框架內容 |



## &lt;map&gt; 與 &lt;area&gt;
可在畫面上製作熱點

### &lt;map&gt;

| 屬姓 | 說明 |
| :--- | :--- |
| name | 名稱 |

### &lt;area&gt;

| 屬性 | 說明 |
| :--- | :--- |
| shape | 熱點形狀 |
| coords="x1,y1,x2,y2" | 熱點座標 |
| href | 熱點的連結文件 |
| nofref | 指定熱點沒有連結文件 |
| alt | 熱點的替換文字 |
| target | 指定目標 |
| hreflang | href語系 |
| rel | href 關聯 |
| type | 內容類型 |
| media | 設備 |

#### 例子

```markup
<body>
    <img src="xxx.jpg" usemap="#myPic">
    <map name="myPic">
        <area shape="circle" coords="173,152,34" href="xxx1.html">
        <area shape="rect" coords="42,159,110,227" href="xxx2.html">
    </map>
</body>
```

