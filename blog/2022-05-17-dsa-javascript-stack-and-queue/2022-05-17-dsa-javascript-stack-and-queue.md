---
slug: dsa-javascript-stack-and-queue
title: 搶過演唱會門票的就懂～來看好用的 Stacks & Queues！
authors: [claudia]
tags: [dsa]
---

此篇為 [Udemy - Master the Coding Interview: Data Structures + Algorithms](https://udemy.com/course/master-the-coding-interview-data-structures-algorithms/?srsltid=AfmBOooCR7IyhxoDQK1cx6-Q*sh7WOI7Q3Z1LnW005R5rxfs5cmnDXo*)
課程筆記。

本篇要來介紹的是 Stacks 和 Queues，這兩個資料結構是比較高層級的資料結構，可以用 Array 或 Linked List 實作出來。

既然可以用 Array 或 Linked List，那為什麼需要像這樣的資料結構呢？原因是 Stacks 和 Queues 可以限制我們使用資料的方式，你只能取得第一個或最後一個，中間的順序一定不會被動到！以下就會示範 Stacks 和 Queues 的特性。

<!-- truncate -->

## Stacks

LIFO（Last In First Out）

首先要介紹的是 Stacks，你可以把 Stacks 想像成疊盤子，一個一個盤子堆疊上去，每次拿都從最上面的那個開始拿，沒有辦法一次抽取最底下或中間的盤子。Stacks 常用到的地方，像是 JavaScript 的 call stack、或是瀏覽器儲存瀏覽紀錄的方式。

Stacks 可以用 Linked List 跟 Array 來實作，以下先用 Linked List 示範：

## Stacks - Linked List

還記得 Linked List 裡面的 head 跟 tail 嗎？這邊把他們改成 top & bottom 會比較好懂。

```jsx
class Node {
  constructor(value){
    this.value = value;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.top = null;
    this.bottom = null;
    this.length = 0;
  }
}

```

### peek()

要查看第一個盤子，直接找 top：

```jsx
peek() {
    return this.top;
}

```

**Big O: O(1)**

### push()

在上面多疊一個盤子，加到最上層（top）：

`2 -> 1`

`3 -> 2 -> 1`

```jsx
push(value) {
    const newNode = new Node(value);
    if (this.length === 0) {
      this.top = newNode;
      this.bottom = newNode;
    } else {
      const holdingPointer = this.top;
      this.top = newNode;
      this.top.next = holdingPointer;
    }
    this.length++;
    return this;
 }

```

**Big O: O(1)**

### pop()

移除第一個盤子（remove top）：

`3 -> 2 -> 1`

`2 -> 1`

```jsx
pop() {
    if (!this.top) {
      return null;
    }
    if (this.top === this.bottom) {
      this.bottom = null;
    }
    const holdingPointer = this.top;
    this.top = this.top.next;
    this.length--;
    return this;
 }

```

**Big O: O(1)**

看完以上 Linked List 實作後，接下來再看看用 Array 實作，因為 JavaScript 已經有 Array 的資料結構，所以相較於 Linked List 會比較簡單：

## Stacks - Array

```jsx
class Stack {
  constructor(){
    this.array = [];
  }
}

```

### peek()

查看第一個（top）：

bottom -> top

`[1, 2, 3]`

```jsx
peek() {
    return this.array[this.array.length-1];
}

```

**Big O: O(1)**

### push()

在最上面多加一個：

bottom -> top

`[1, 2, 3] -> [1, 2, 3, 4]`

```jsx
push(value) {
    this.array.push(value);
    return this;
}
```

**Big O: O(1)**

### pop()

移除最上面的一個：

bottom -> top

`[1, 2, 3, 4] -> [1, 2, 3]`

```jsx
pop() {
    this.array.pop();
    return this;
}
```

**Big O: O(1)**

## Queues

FIFO (First In First Out)

接著第二種資料結構是 Queues，顧名思義他的運作方式，就是真的是跟平常排隊一樣，遵循先來後到的規定。

Queues 常被運用的地方，像是大家搶演唱會門票時，在網站上排隊，或是印表機有很多的 task 的時候，依序處理。

不像 Stacks 可以用 Linked List 跟 Array 來實作，Queues 最好不要用 Array 來實作，原因是排隊的情況，第一個人進去以後，後面的會需要遞補上來，用 Array 實作會要移動到 index，也就會造成 O(n) 的運算，相對於 Linked List O(1) 的運作顯得麻煩許多。

## Queues - Linked List

這裡把 Linked List 的 head & tail 命名成 first & last：

```jsx
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class Queue {
  constructor(){
    this.first = null;
    this.last = null;
    this.length = 0;
  }
}

```

### peek()

查看排在第一個的人：

```jsx
peek() {
    return this.first;
}
```

**Big O: O(1)**

### enqueue()

跟排隊一樣，所以加在最後一個。

`1 -> 2 -> 3`

`1 -> 2 -> 3 -> 4`

```jsx
enqueue(value) {
    const newNode = new Node(value);
    if (this.length === 0) {
      this.first = newNode;
      this.last = newNode;
    } else {
      this.last.next = newNode;
      this.last = newNode;
    }
    this.length++;
    return this;
 }
```

**Big O: O(1)**

### dequeue()

跟排隊一樣，放第一個進去。

`1 -> 2 -> 3 -> 4`

`2 -> 3 -> 4`

```jsx
dequeue() {
    if (!this.first) {
      return null;
    }
    if (this.first === this.last) {
      this.last = null;
    }
    const holdingPointer = this.first;
    this.first = this.first.next;
    this.length--;
    return this;
}

```

**Big O: O(1)**

## Pros & Cons

- Pros
    
1. Fast operations
2. Fast peek
3. Ordered
    
- Cons
    
1. Slow lookup O(n)


## Reference

[Udemy - Master the Coding Interview: Data Structures + Algorithms](https://udemy.com/course/master-the-coding-interview-data-structures-algorithms/?srsltid=AfmBOooCR7IyhxoDQK1cx6-Q*sh7WOI7Q3Z1LnW005R5rxfs5cmnDXo*)
