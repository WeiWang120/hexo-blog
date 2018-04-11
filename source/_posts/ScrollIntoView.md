---
title: ScrollIntoView
date: 2018-04-11 15:04:54
tags: [javascript, 浏览器兼容, DOM]
---


### Scroll Into View

假设我有
```html
  <button id="go_to">Go to 3</button>
  <!-- content height 固定，overflow：scroll -->
  <div id="content">
    <div id="anchor1">1</div>
    <div id="anchor2">2</div>
    <div id="anchor3">3</div>
    <div id="anchor4">4</div>
    <div id="anchor5">5</div>
    <div id="anchor6">6</div>
  </div>
```
```javascript
  document.getElementById('go_to').addEventListener('click', handleClick)
```

要实现点击按钮然后滚动到相应 anchor ，最简单的方法是

```
  const handleClick = () => {
    location.href = '#anchor'
  }
```

但是这样页面会直接跳转到相应位置，为了让页面能滚动到而不是直接跳到该地方，我们可以用

```javascript
document.getElementById('anchor').scrollIntoView({
  behavior: 'smooth'
})
```

但是该方法在 Safari 和 IE 中并不能实现滚动特效的兼容，只会跳到 anchor，详见 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView)


用下面这个方法可以做到浏览器的兼容
```javascript
  // element 是需要 scroll 的元素，to 指的是向下滚多少距离，duration 是时间
  const scrollTo = (element, to, duration) => {
    let currentTime
    const start = element.scrollTop
    const change = to - start
    const increment = 20
    currentTime = 0        
    const animateScroll = () => {   
        currentTime += increment
        const val = easeInOutQuad(currentTime, start, change, duration)
        element.scrollTop = val
        if(currentTime < duration) {
          setTimeout(animateScroll, increment)
        }
    }
    animateScroll()
  }

  //t = current time
  //b = start value
  //c = change in value
  //d = duration
  const easeInOutQuad = (t, b, c, d) => {
      t /= d/2
      if (t < 1) return c/2*t*t + b
      t--
      return -c/2 * (t*(t-2) - 1) + b
  }
```

这样当需要滚动到例如 3 的时候，element 为 content，to 为当 content 滚动到3的时候 element.scrollTop。
```javascript
  const handleClick = () => {
    const content = document.getElementById('#content')
    // 要拿到滚动到3的 content.scrollTop
    const anchor3 = document.getElementById('#anchor3')
    const to = anchor3.getBoundingClientRect().top + content.scrollTop // 第一项为 anchor3 距离 viewport 的距离， 第二项是当前 content 的 scrollTop
    scrollTo(content, to, 1000)
  }
```








