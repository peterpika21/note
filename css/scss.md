###### tags: `CSS`
# SCSS

## 變數

在建置網頁時，美術設計師都會設計數組文字連結顏色， 像上面就是在各個div裡面的設定文字顏色都不一樣， 所以當如果我要修改的時候就必須逐行去瀏覽並修正， 雖然可以透過一些工具全選並替換字串， 但也容易會漏掉一些少設定的code

```sass=
$link-color-1: #000
$link-color-2: blue
.header a
  color: $link-color-1
.menu a
  color: $link-color-2
.footer a
  color: $link-color-1
.nav a
  color: $link-color-2
```

```sass=
//常用的全域設定就有下述幾種
$text-color: pink //預設文字顏色  
$link-color: #000 //文字連結樣式，多則就再加紹數字
$link-color-hover: #fff //滑鼠拖曳過後的樣式顏色  
$font-size: 13px //全域字型大小，如一網站有多種就下多種字型
$line-height: 1.8 //全域行距  
$container-width: 960px //網站整體寬度
$font-style："Helvetica Nueue", Arial, Sans Serif;  全域字型
```

## mixin

解決重複寫相同程式碼問題

```sass=
// 創建 mixin
@mixin bg($bgcolor){
  backgroung: $bgcolor;
}

// 使用 mixin
@include bg(red);
```

## extend
繼承樣式  (不建議拉出 global extend 包，開發後期容易難以辨識)

```sass=
// 創建
%all-h1 {
  font-size: 20px
  line-height: 1.8
  letter-spacing: 1px
}

// 使用
.demo {
  @extend %all-h1
  color:red;
  ...
}
```


## 顏色函數
只介紹兩個最常用到的

```sass=
// 調暗
.demo{
  color: darken(顏色,數值(%));
}

// 調亮
.demo{
  color: lighten(顏色,數值(%));
}
```

## 迴圈

### each

循環絕對是Sass提供的三個循環方式中最常用的。它提供了一個簡潔的 API 來迭代列表或 map

```sass=
@each $theme in $themes {
  .section-#{$theme} {
    background-color: map-get($colors, $theme);
  }
}
```

當迭代一個 map 時，通常使用 $key 和 $value 作為變量名稱來確保一致性

```sass=
@each $key, $value in $map {
  .section-#{$key} {
    background-color: $value;
  }
}
```

### for

當需要聚合偽類 :nth-\* 的時候，使用 @for 循環很有用。除了這些使用場景，如果_必須_迭代最好還是使用 @each 循環

```sass=
@for $i from 1 through 10 {
  .foo:nth-of-type(#{$i}) {
    border-color: hsl($i * 36, 50%, 50%);
  }
}
```


## map
等同於 object

基礎架構

```sass=
$map: (
  key1: value1, 
  key2: value2, 
  key3: value3
);
```

1.function

```sass=
1.map-get($map,$key) :取出指定的值
2.map-merge($map1, $map2) :合併多個map
3.map-remove($map, $keys…) 移除指定的值
```

demo

```sass=
// 先定義
$font:(
  h1: 36px,
  h2: 32px,
  h3: 30px,
  h4: 24px,
  h5: 20px
)

// 搭配 @each 使用
@each $name,$value in $font {
  #{$name}{
    font-size: $value;
  }
}
```

## mixin與if

與 if 搭配使用 , mixin 更靈活 如 demo

```sass=
//預設是等腰三角形，如要做正三角形，請將type預設變數改為0.8666666 
@mixin triangle ($size, $color, $align,$type:1)
  height: 0
  width: 0
  @if $align == top 
    border-bottom: ($size*$type) solid $color
    border-left: ($size/2) solid transparent
    border-right: ($size/2) solid transparent
  @else if $align == right 
    border-left: ($size*$type) solid $color
    border-top: ($size/2) solid transparent
    border-bottom: ($size/2) solid transparent
  @else if $align == bottom 
    border-top: ($size*$type) solid $color
    border-left: ($size/2) solid transparent
    border-right: ($size/2) solid transparent
  @else if $align == left 
    border-right: ($size*$type) solid $color
    border-top: ($size/2) solid transparent
    border-bottom: ($size/2) solid transparent
  @else if $align == right-top
    border-right: $size solid $color
    border-bottom: $size solid transparent
  @else if $align == left-top
    border-left: $size solid $color
    border-bottom: $size solid transparent
  @else if $align == right-bottom
    border-right: $size solid $color
    border-top: $size solid transparent
  @else if $align == left-bottom
    border-left: $size solid $color
    border-top: $size solid transparent  
.box
  +triangle(30px,red,right)
```


## RWD
響應式設計，這邊教學使用 @content建構 RWD

```sass=
// 使用 mixin 註冊斷點
@mixin breakpoint($point) {

  // 判斷變數是pc、pad還是mobile，再依照判斷出來的載入對應的media query
  @if $point=="pc" {
    @media only screen and (max-width: 1024px) {
      @content;
    }
  }

  @else if $point=="pad" {
    @media only screen and (max-width: 768px) {
      @content;
    }
  }

  @else if $point=="mobile" {
    @media only screen and (max-width: 320px) {
      @content;
    }
  }
}

.one {
  width: 600px
  @include breakpoint("pc") {
    width: 300px
    height: 50px
  }
  @include breakpoint("pad") {
    width: 100%
  }
}
```


## index\(\)與nth\(\)
變數也可以設定多值

1. **nth\(\)**

```sass=
$text-color: #ffffff,orange,yellow
.content{
  color:nth($text-color,2) //引入變數,第幾個
}
```

2. **index\(\)**

```sass=
$site: coffee,baseball,dvd
.content{
  color:index($site,dvd) //引入變數,變數名稱
}

// result
.content{
  color: 3;
}
```

合用

```sass=
$sites:coffee,cart,cloth,tea,child   //網站類型
$text-color:red,orange,yellow,green,blue //文字顏色
$bg:#fff,#000,#000,gray,#fff  //背景顏色
$style: index($sites,coffee)  //選擇coffee後 = $style:1
.box{
  background: nth($bg,$style)
  color: nth($text-color,$style)
}
```


## transition
能讓效果更有感覺

漸變效果

linear：平均速度 

ease：很快到漸慢 

ease-in：逐漸變快 

ease-out：逐漸變慢 

ease-in-out：快→慢→快

cubic-bezier\(\):

用法

```sass=
transition: all 1s cubic-bezier(0.250, 0.250, 0.750, 0.750)　　//平均速度
```


## 7-1模式
7個文件夾 + 1檔案

理想資料結構如下

```sass=
sass/
|
|– base/
|   |– _reset.scss       # Reset/normalize
|   |– _typography.scss  # Typography rules
|   ...                  # Etc…
|
|– components/
|   |– _buttons.scss     # Buttons
|   |– _carousel.scss    # Carousel
|   |– _cover.scss       # Cover
|   |– _dropdown.scss    # Dropdown
|   ...                  # Etc…
|
|– layout/
|   |– _navigation.scss  # Navigation
|   |– _grid.scss        # Grid system
|   |– _header.scss      # Header
|   |– _footer.scss      # Footer
|   |– _sidebar.scss     # Sidebar
|   |– _forms.scss       # Forms
|   ...                  # Etc…
|
|– pages/
|   |– _home.scss        # Home specific styles
|   |– _contact.scss     # Contact specific styles
|   ...                  # Etc…
|
|– themes/
|   |– _theme.scss       # Default theme
|   |– _admin.scss       # Admin theme
|   ...                  # Etc…
|
|– utils/
|   |– _variables.scss   # Sass Variables
|   |– _functions.scss   # Sass Functions
|   |– _mixins.scss      # Sass Mixins
|   |– _helpers.scss     # Class & placeholders helpers
|
|– vendors/
|   |– _bootstrap.scss   # Bootstrap
|   |– _jquery-ui.scss   # jQuery UI
|   ...                  # Etc…
|
|
`– main.scss             # Main Sass file
```

1. BASE

文件夾存放項目中的模板文件。在這裡，可以找到重置文件、排版規範文件或者一個樣式表——定義一些 HTML 元素公認的標準樣式

* `_reset.scss`
* `_typography.scss`

2. LAYOUT

文件夾存放構建網站或者應用程序使用到的佈局部分。該文件夾存放網站主體（頭部、尾部、導航欄、側邊欄…）的樣式表、柵格系統甚至是所有表單的 CSS 樣式。

* `_grid.scss`
* `_header.scss`
* `_footer.scss`
* `_sidebar.scss`
* `_forms.scss`
* `_navigation.scss`

3. COMPONENTS 或 MODULES

局部組件樣式

* `_media.scss`
* `_carousel.scss`
* `_thumbnails.scss`

4. PAGES

特定樣式頁面

* `_home.scss`
* `_contact.scss`

5. THEMES

大型網站存放主題樣式 \(許多項目用不到\)

* `_theme.scss`
* `_admin.scss`

6. UTILS 或  HELPERS

文件夾包含了整個項目中使用到的 Sass 輔助工具，這裡存放著每一個全局變量、函數、混合宏和占位符。

該文件夾的經驗法則是，編譯後這裡不應該輸出任何 CSS，單純的只是一些 Sass 輔助工具。

* `_variables.scss`
* `_mixins.scss`
* `_functions.scss`
* `_placeholders.scss`

7. VENDORS

存放外部代碼

* `_normalize.scss`
* `_bootstrap.scss`
* `_jquery-ui.scss`
* `_select2.scss`

8.main.scss \(入口文件\)

依照順序引用

1. `vendors/`
2. `utils/`
3. `base/`
4. `layout/`
5. `components/`
6. `pages/`
7. `themes/`

範例

```sass=
@import 'vendors/bootstrap';
@import 'vendors/jquery-ui';

@import 'utils/variables';
@import 'utils/functions';
@import 'utils/mixins';
@import 'utils/placeholders';

@import 'base/reset';
@import 'base/typography';

@import 'layout/navigation';
@import 'layout/grid';
@import 'layout/header';
@import 'layout/footer';
@import 'layout/sidebar';
@import 'layout/forms';

@import 'components/buttons';
@import 'components/carousel';
@import 'components/cover';
@import 'components/dropdown';

@import 'pages/home';
@import 'pages/contact';

@import 'themes/theme';
@import 'themes/admin';
```

