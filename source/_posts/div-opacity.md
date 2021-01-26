---
title: 實作父層 div 有透明效果，子層 div 無透明
date: 2019-06-06
categories:
- [CSS]
---
### CSS 屬性 opacity 簡介
CSS opacity 屬性可讓 DOM 具有透明的效果，value 範圍 0 - 1，數值越低透明度越高。

要注意的是，如果父層元素設有 opacity 屬性，效果會影響底下所有的子元素，例如父層設定 opacity 為 0.5，則子元素也具有 opacity 0.5 的透明效果，即使另外定義子元素的 opacity 為 1，也無法 override 父層的 opacity。

如果今天有個需求，需要建立一個外層 div 有透明，但是內層 div 無透明的元件，該如何實作呢？

此時就需要用到 RGBA 色彩空間，不需要在父層設定 opacity，直接使用 RGBA 設定父層 background color，利用第四個參數 alpha 設定透明度，就不會影響到子元素了。

例如：
```HTML
// HTML
<div class="basic box1">    
  <div class="inside-box">受父層 opacity 影響 </div>
</div>

<div class="basic box2">
  <div class="inside-box">無 opacity 效果的 BOX</div>
</div>
```
```CSS
// CSS
.basic {
  position: absolute;
  height: 500px;
  width: 500px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.box1 {
  background: red;
  opacity: 0.7;
}

.box1 .inside-box {
  opacity: 1;
}

.box2 {
  left: 520px;
  background: rgba(0, 0, 0, 0.5);
}

.inside-box {
  height: 200px;
  width: 200px;
  margin: 0 auto;
  background: blue;
  color: yellow;
  text-align: center;
  line-height: 200px;
}
```

結果：
{% asset_img div-opacity.png %}

