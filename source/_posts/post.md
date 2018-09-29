---
title: Implement Queue, Stack, PriorityQueue
date: 2018-09-29 00:50:35
tags:
---

### 1. Queue

```javascript
class Queue {
  constructor () {
    this.queue = []
  }

  offer (value) {
    this.queue.push(value)
  }

  isEmpty () {
    return this.size() === 0
  }

  size () {
    return this.queue.length
  }

  poll () {
    return this.queue.shift()
  }

  peek () {
    return this.queue[0]
  }
}
```

### 2. Stack

```javascript
class Stack {
  constructor () {
    this.queue = []
  }

  push (value) {
    this.queue.push(value)
  }

  isEmpty () {
    return this.size() === 0
  }

  size () {
    return this.queue.length
  }

  pop () {
    return this.queue.pop()
  }

  peek () {
    return this.queue[this.size() - 1]
  }
}
```

### 3. PriorityQueue

```javascript
class PriorityQueue {
  constructor (comparator = (a, b) => a - b) {
    this.queue = [null]
    this.comparator = comparator
  }

  isEmpty () {
    return this.size() === 0
  }

  size () {
    return this.queue.length - 1
  }

  offer (value) {
    this.queue.push(value)
    let valueIndex = this.size()
    let parentIndex = Math.floor(valueIndex / 2)
    // if the parent exists and satisfy the comparator
    while (this.queue[parentIndex] && this.comparator(this.queue[valueIndex], this.queue[parentIndex]) < 0) {
      // swap parent with child
      const parent = this.queue[parentIndex]
      this.queue[parentIndex] = this.queue[valueIndex]
      this.queue[valueIndex] = parent
      // new value index will be parent index
      valueIndex = parentIndex
      parentIndex = Math.floor(valueIndex / 2)
    }
  }

  poll () {
    if (this.isEmpty()) return null
    if (this.size() === 1) {
      return this.queue.pop()
    }
    const toBeReturn = this.queue[1]
    this.queue[1] = this.queue.pop()
    let valueIndex = 1
    let [left, right] = [valueIndex * 2, valueIndex * 2 + 1]
    let swapIndex = this.queue[right] && this.comparator(this.queue[right], this.queue[left]) <= 0 ? right : left
    while (this.queue[swapIndex] && this.comparator(this.queue[valueIndex], this.queue[swapIndex]) >= 0) {
      const swap = this.queue[swapIndex]
      this.queue[swapIndex] = this.queue[valueIndex]
      this.queue[valueIndex] = swap
      valueIndex = swapIndex
      left = valueIndex * 2
      right = valueIndex * 2 + 1
      swapIndex = this.queue[right] && this.comparator(this.queue[right], this.queue[left]) <= 0 ? right : left
    }
    return toBeReturn
  }

  peek () {
    return this.queue[1]
  }
}
```
