---
title: Promise 和 Fetch API
date: 2018-01-30 18:51:30
tags: javascript
---
这两天看了很多关于 Promise 和 fetch 的文档，想整理一下以备日后复习。

### 1. Promise
什么是 Promise 呢，在 MDN 上给出的定义是这样的

The **Promise** object represents the eventual completion (or failure) of an **asynchronous** operation, and its resulting value.

关键词是 asynchronous，Promise 让异步方法可以像同步方法一样返回值。

在 Promise 出现在 ES6 标准之前，传统的异步操作是这样的
```javascript
  const wait = (callback) => {
    const task = () => {
      console.log('done')
      callback(true)
    }
    setTimeout(task, 5000)
  }
  wait(bool => {
    if (bool) console.log('success')
    else console.log('error')
  })
```
使用 Promise 封装之后是这样的
```javascript
  const wait = () => {
    return new Promise((resolve, reject) => {
      const task = () => {
        console.log('done')
        resolve('success')
      }
      setTimeout(task, 5000)
    })
  }
```
这时候执行 `wait()` 返回的是一个 `promise`， 我们可以用 `then` 来执行后续的操作

```javascript
  wait().then((res) => {
    console.log(res) // success
  })
```

加上 ES7 的 async/await ，我们可以让代码可读性更高， 如同同步一般

```javascript
// 外层包一个 async function
  try {
    const res = await wait()
    console.log(res) // success
  } catch (e) {
    console.log('Oops', e)
  }
```

另外最近学到了一个关于 Promise 很巧妙的用法！需求是这样的，一段 js 代码中 `ReactDOM.render(<SomePage/>)` ，在这个 `SomePage` 中若用户进行了某项操作，则继续执行 js 后面的代码，否则一直卡在当前界面。

这个时候就可以用 Promise 来实现这个需求，将 `resolve` 作为 `props` 传进 `SomePage`， 当用户进行某项操作后（比如点击）执行 resolve 于是 js 文件中后面的代码继续执行

```javascript
  // 外层包一个 async function
  await new Promise((resolve) => {
    ReactDOM.render(<SomePage continueRun={resolve} />)
  })

  // 后续代码...
```

### 2. Fetch

Fetch API 是基于 Promise 设计的，使用 fetch 返回的是一个 Promise 对象

```javascript
  try {
    const response = await fetch(url)
    console.log(res)
  } catch (e) {
    console.log(e)
  }
```

fetch 的优点是

1. 语义简介，基于 Promise 实现
2. 可以使用 async/await 可读性好

缺点是

1. 请求默认不带 cookie
2. 是一个 low-level API，最好自己根据需求进行封装

下面简单封装一个 fetchJson 的方法

```javascript
  import merge from 'lodash/merge'

  const fetchJson = async (url, options) => {
    const default = {
      method: 'GET'
      crendentials: 'include'  // 带上cookie
      headers: { 'Content-Type': 'application/json' }
    }
    const request = new Request(url, merge({}, default, options))
    return fetch(request)
  }
```

这样 fetchJson 默认是用 GET 方法去获取 Json 数据，若需要改变 method 为 PUT
只需要

```javascript
  fetchJson(url, { method: 'PUT' })
```

以上只是自己目前一点浅显的理解，日后还会不定期更新~
