---
title: 來寫一個密碼產生器吧
date: 2021-01-28
categories:
- [JavaScript]
---

### 程式概念

使用網頁前端基本技術 HTML, CSS, JavaScript 實作，練習目的放在 DOM 的操作上和 JS 的使用，UI 部分則使用 Bootstrap 5，介面簡單優雅即可，本文後續重點會放在 JS 的設計想法。

### 程式介面
{% asset_img passwordGenerator.jpg '"密碼產生器"' %}

<!-- more -->

### 功能說明

表單共有 6 個欄位。

第一個項目 `input` 欄位決定密碼長度，輸入值必須介於 4 - 16 之間。  

第二到第五個項目為勾選框 `checkbox`，決定密碼的組成元素，包括英文字母大小寫、數字與特殊符號。

第六個項目 `input` 欄位提供使用者自行輸入字元，輸入的字元將不會出現在產生的密碼字串裡。

### 程式邏輯

首先使用瀏覽器提供的 API `getElementById` 取得表單欄位和 `button` DOM，並在產生密碼按鈕 `generatePasswordBtn` 監聽 `click` 事件綁定自定義函式 `generatePassword`

``` javascript
const passwordLength = document.getElementById('passwordLength')
const lowercaseCheck = document.getElementById('lowercaseCheck')
const uppercaseCheck = document.getElementById('uppercaseCheck')
const numberCheck = document.getElementById('numberCheck')
const symbolCheck = document.getElementById('symbolCheck')
const excludeInput = document.getElementById('excludeInput')
const resultText = document.getElementById('passwordResult')
const generatePasswordBtn = document.getElementById('generateBtn')

generatePasswordBtn.addEventListener('click', generatePassword)
```

接下來討論如何產生隨機字符，有以下 2 種想法：

第一種是先定義已經包含所有字符的字串，再產生隨機數字使用 `indexOf(n)` 取得字符串接產生最終的密碼字串。

第二種是參考 [ASCII Code](https://www.ascii-code.com/) 表，同樣產生隨機數字後使用 JS 內建的 `String.fromCharCode(n)` 取得對應的字符，串接產生最終密碼字串。

這邊使用第二種方法實現。

首先定義產生隨機數字和產生字符的函示：

``` javascript
// 產生 33 到 126 之間的隨機整數
function createRandomNumber() {
  return Math.floor(Math.random() * 94) + 33
}

// 傳入數字，回傳 fromCharCode(n) 產生的字符
function getCharacter(number) {
  // 這邊傳入的參數 number 會是陣列，使用 Spread syntax ...
  // 一次傳入多個參數，這樣只要呼叫一次函示即可回傳最終的密碼字串
  return String.fromCharCode(...number)
}
```

到這邊我們已經可以產生密碼了，只要根據輸入的數字決定迴圈次數，使用 `createRandomNumber()` 產生隨機數字陣列，再當作參數傳入 `getCharacter()` 即可取得密碼字串。

但是程式功能必須可以自定義密碼包含的字符樣式，例如定義密碼不得含有大寫英文字母，因次不是每次產生的隨機數字都可以拿來使用，我們再額外定義函示 `getCharCode()`，根據表單裡勾選的 `checkbox` 來決定是否要使用這個數字。

``` javascript
function getCharCode() {
  // 取得自行定義的例外字符
  const excludeStr = excludeInput.value
  // 產生隨機數字
  const num = createRandomNumber()
  // 檢查 num 對應的字符是否有在例外字符字串裡
  // 有的話再呼叫函示本身，重新跑一次流程
  if (excludeStr.indexOf(getCharacter([num])) > -1) {
    return getCharCode()
  }
  // 檢查是否包含小寫英文字母
  // 有勾選的話檢查 num 是否在小寫字母的 ASCII Code 範圍內
  if (lowercaseCheck.checked) {
    if (num >= 97 && num <= 122) {
      return num
    }
  }
  // 檢查是否包含大寫英文字母
  // 有勾選的話檢查 num 是否在大寫字母的 ASCII Code 範圍內
  if (uppercaseCheck.checked) {
    if (num >= 65 && num <= 90) {
      return num
    }
  }
  // 檢查是否包含數字 0 - 9
  // 有勾選的話檢查 num 是否在數字的 ASCII Code 範圍內
  if (numberCheck.checked) {
    if (num >= 48 && num <= 57) {
      return num
    }
  }
  // 檢查是否包含特殊字元
  // 有勾選的話檢查 num 是否在特殊字元的 ASCII Code 範圍內
  if (symbolCheck.checked) {
    if (
      num >= 33 && num <= 47 ||
      num >= 58 && num <= 64 ||
      num >= 91 && num <= 96 ||
      num >= 123 && num <= 126
    ) {
      return num
    }
  }
  //  都不符條件的話則再呼叫函示本身，重新跑一次流程
  return getCharCode()
}
```

最後定義 `generatePassword()` 函示

``` javascript
function generatePassword() {
  // 檢查密碼長度
  if (
    !passwordLength.value ||
    Number(passwordLength.value) < 4 ||
    Number(passwordLength.value) > 16
  ) {
    resultText.innerText = 'Password length should between 4 and 16.'
    return
  }
  // 檢查是否有勾選至少一項密碼格式
  if (
    !lowercaseCheck.checked &&
    !uppercaseCheck.checked &&
    !numberCheck.checked &&
    !symbolCheck.checked
  ) {
    resultText.innerText = 'You must select at least one character set.'
    return
  }
  // 產生數字陣列
  const arr = []
  for (let i = 0; i <= Number(passwordLength.value); i++) {
    arr.push(getCharCode())
  }
  // 產生最終密碼字串
  resultText.innerText = getCharacter(arr)
}
```

最終成果可在 [CodePen](https://codepen.io/yamsiu/pen/ZEBzXgP) 上使用，完整程式碼可參考 [GitHub](https://github.com/iamsiutw/Password-Generator)
