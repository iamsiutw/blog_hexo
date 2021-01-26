---
title: calc() 運算
date: 2019-04-16
categories:
- [CSS]
---

### calc( ) 簡介
calc( )可使用在 CSS 中運算數值，包含基本的加減乘除，而且屬性值還不用相同單位，方便開發者不用為了微調 UI 而計算數值到天荒地老。通常會用在 width 和 height 屬性。

例如：
```SCSS
.test {
    width: calc(300px - 1rem);
}
```

### SCSS 使用變數預處理器
在 SCSS 格式的樣式檔案中，當有個特定的顏色或是數值很常被重複使用時，可以使用 $ 符號加 valuableName 組成變數，之後便可直接使用這個變數作為屬性值，如果未來遇到需要更換的情況，直接修改該變數，底下所以使用該變數的樣式便會套用新的屬性值。

例如：
```SCSS
$primary-width: 100px;

.area1 {
    width: $primary-width;
    height: 50px;
}

.area2 {
    width: 50px;
    height: $primary-width;
}
```

### calc( ) 內使用 SCSS 變數
如果要在 calc( ) 中使用變數需特別注意，變數名稱必須使用 #{ } 包起來，calc( ) 才能正確運行。

例如：
```SCSS
$primary-width: 100px;

.area {
    width: calc(#{$primary-width} - 10rem);
    height: 50px;
}
```
