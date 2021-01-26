---
title: 修改 input 的 placeholder 屬性顏色
date: 2019-04-22
categories:
- [CSS]
---
### input placeholder 簡介
**input** 是前端經常會使用到的 tag ，可讓使用者輸入內容與網站產生互動。

**placeholder** 是 **input** 的其中一項 property，讓 input 在 DOM 上產生偽元素文字作為提示，當 input 有 value 時即會消失。

**::placeholder** 選擇器讓開發者直接修改 plaerholder 的顏色。

例如：
```SCSS
// HTML

<div>
  <input class="colored" type="text" placeholder="紅色的 placeholder">
  <br>
  <br>
  <input type="text" placeholder="預設的 placeholder">
</div>

// CSS
.colored::placeholder {
  color :red;
}

```

結果：
{% asset_img placeholder.png %}
