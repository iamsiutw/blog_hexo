---
title: 邏輯運算子與布林值使用技巧
date: 2020-01-07
categories:
- [JavaScript]
---

### !! - Double NOT 運算子
**!val**

邏輯運算子 `!` 可將 truthy 與 falsy 的變數轉化為對向的 Boolean，同理可以直接使用 `!!` 將變數轉化為 Boolean。

例如：
``` javascript
const someInput = ''
const isValidInput = !!someInput
console.log(isValidInput) // false
```

### || - OR 運算子
**val1 || val2**

邏輯運算子 `||` 判斷 val1 為 truthy 則返回 val1， falsy 則返回 val2。

例如：

``` javascript
const name1 = ''
const name2 = 'Wang'
const defalutName1 = name1 || 'Huang'
const defalutName2 = name2 || 'Huang'
console.log(defalutName1) // Huang
console.log(defalutName2) // Wang
```

### && - AND 運算子
**val1 && val2**

邏輯運算子 `&&` 判斷 val1 為 falsy 則返回 val1， truthy 則返回 val2。

例如：

``` javascript
const log1 = true
const log2 = false
const user1 = log1 && 'Chen'
const user2 = log2 && 'Hu'
console.log(user1) // Chen
console.log(user2) // false
```
