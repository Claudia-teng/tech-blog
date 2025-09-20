---
slug: dsa-javascript-array
title: 用 JavaScript 實作 Array 資料結構
authors: [claudia]
tags: [dsa]
---

首篇 Data Structure 想要介紹的是 Array，在 JavaScript 當中，因為已經有內建的 Array 資料結構，因此平常已經習慣`let array = [];`這種寫法。

不過 JavaScript 的 Array 其實也是從 Object 衍生而來，如果今天在 console 輸入`typof []`，你會得到`'object'`的答案，原因是在 JavaScript 中，Array 算是特別的 Object。

Array 資料結構其實也能透過 JavaScript 自己實作，以下將會示範怎麼透過 Object 來實作 Array，以及自己寫出一些常見的 Array Method。

接下來就一起來實作吧！

### Array

首先建立一個`Class`叫做 MyArray，並在`constructor`內預設兩個值：length, data，作為空陣列。

```jsx
class MyArray {
  constructor() {
    this.length = 0;
    this.data = {};
  }
}
```

接下來我們要在 MyArray 裡面加入一些 method：

### Get

先來實作如何藉由 index，取得 Array 裡相對應的值，使用的是`Object[key]`的方式。

```jsx
get(index) {
  return this.data[index]
}
```

Big O: O(1)

Array 因為有 index，因此搜尋速度很快。

### Push

Array.push()是在 array 的最後面加一個值，並回傳整個 array，實作如下：

```jsx
push(item) {
  this.data[this.length] = item;
  this.length++;
  return this.data;
}
```

Big O: O(1)

通常為 O(1)，只需在最後加上值，不用改變整個結構。但 Array 在 RAM 儲存值的方式為連續的，若 Array 末端沒有空間放最後一個值，則須將整個 Array 搬移到 RAM 其他處，此時 Big O: O(n)。

### Pop

Array.pop()是移除 array 最後一個值，並回傳被移除的值，實作如下：

```jsx
pop() {
  const lastItem = this.data[this.length - 1];
  delete this.data[this.length - 1];
  this.length--;
  return lastItem;
}
```

這裡使用到的是 Object 的刪除方式`delete Object[key]`。

Big O: O(1)

只需移除最後一個值，不用改變其他結構。

### Delete

最後來實作如何移除 Array 中特定 index 的值，並回傳被移除的值。

這邊實作的方法是，將給予的 index 值換成 index+1 的值。

例如：

```jsx
let array = [45, 26, 33, 42, 55];
// index      0   1   2   3   4
```

若我今天想移除 index 3 (值為 42)，我必須將 index 3 所對應的值換為 55 (也就是原本的 index 4)，將後面一個替補上去的意思。

```jsx
let array = [45, 26, 33, 42, 55];
// index      0   1   2   3   4
```

而移除後也要記得將最後一個值刪除。

```jsx
let array = [45, 26, 33, 55];
// index     0   1   2   3
```

實作如下：

```jsx
deleteAtIndex(index) {
  const item = this.data[index];
  for (let i = index; i < this.length - 1; i++) {
    // this.length - 1 是因為array最後一個index不用做替補的動作
    this.data[i] = this.data[i + 1];
  }
  // 刪除最後一個
  delete this.data[this.length - 1];
  this.length--
  return item;
}
```

或是比較簡潔的寫法，把中間的過程移出來變成一個 function。

```jsx
deleteAtIndex(index) {
  const item = this.data[index];
  this.shiftItems(index);
  return item;
}

shiftItems(index) {
  for (let i = index; i < this.length - 1; i++) {
    this.data[i] = this.data[i + 1];
  }
  delete this.data[this.length - 1];
  this.length--;
}

```

Big O: O(n)

index 之後的結構都會改變，又因 Big O 考慮的是最差情況，若 index = 0，整個 Array 結構都須改變，因此為 O(n)。

最後即可呼叫 MyClass 的 method：

```jsx
const myArray = new MyArray();

myArray.push("Hellooo");
myArray.push(",");
myArray.push("Nice");
myArray.push("to");
myArray.push("meet");
myArray.push("you");
myArray.pop();
myArray.deleteAtIndex(0);
myArray.push("!");
myArray.shiftItems(0);
```

以上就是自己用 JavaScript Object 實作 Array 資料結構，和一些簡單的 method 的方式！

### Pros & Cons

1. Pros

   Good at having sorted data (ordered)

   Fast lookup

   Fast push/pop

   Ordered

2. Cons

   Slow Search (find value) O(n)

   Slow inserts O(n)

   Slow deletes O(n)
