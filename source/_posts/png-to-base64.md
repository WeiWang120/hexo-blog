---
title: png to base64
date: 2018-04-16 13:56:34
tags: [canvas, image]
---

### Png 图片转换成 base64

上周新写了一个网页，上面有许多的图标，最开始我把图标都通过 `<img src='/path/to/icon' />` 的方式引了进来，但是这样页面加载的时候有大量 http 请求，相应的需要更长的加载时间。并且每一张图片只有几 kb 大小，这样使用大量 http 请求未免成本太高。


后来准备用 css sprites 把这些小的图标打成一张大图，然后通过 background-position 来显示每一个图标，这样一来对于众多图标只需要进行一次 http 请求，感觉性能瞬间提升。但是如果后期又需要增加新的图标呢，或者是要更换现有的图标呢？那又得重新生成一次大图，并且还要在原来的大图里小图的顺序不变的情况下。这样一来这个方法又显得不那么完美了，后期不是很好维护。


最后选择把所有 Png 转换成 base64 的格式，把资源直接镶嵌在页面中，这样每一张图都可以单独维护，并且大量减少 http 请求数，缺点就是 base64 编码会增加1/3大小，但是对于较小的图片来说是可以接受的。把 png 转换成 base64 可以使用网上的工具 [base64](https://www.base64-image.de/), 但是这样一行行的复制粘贴 base64 编码效率太低，于是用 canvas 写了一个 png 转换为 base64 并且输出自己需要的格式的脚本


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>converter</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
    }
    #drag_drop {
      margin: auto;
      width: 500px;
      height: 500px;
      /*background: green;*/
      border: 1px solid black;
      text-align: center;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    li {
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div id="drag_drop">drag images over here</div>
  <canvas id="myCanvas" style="display: none"></canvas>
  <div id="base_64">
  </div>
  <script>
    var array = []
    const drag_drop = document.querySelector('#drag_drop')
    drag_drop.addEventListener('dragover', (e) => e.preventDefault())
    drag_drop.addEventListener('drop', (e) => {
      e.preventDefault()
      loadImages(e.dataTransfer.files)
    })

    const loadImages = async (files) => {
      const json = await generateJson(files)
    }

    const generateJson = (files) => {
      return new Promise(resolve => {
        const json = {}
        for (var key in files) {
          const file = files[key]
          if (typeof file === 'object') {
            if (file.type.indexOf('image') === -1) {
              console.log("It's not an image")
              return
            }
            const name = file.name
            const reader = new FileReader()
            reader.onload = async (e) => {
              const res = await render(e.target.result)
              // generate json
              json['name'] = name
              json['logo_path'] = res
              array.push(JSON.stringify(json))
              const base_64 = document.getElementById('base_64')
              base_64.innerHTML = array.toString()
            }
          
            reader.readAsDataURL(file)
          } 
        }
        resolve(json)
      })
    }

    const render = (src) => {
      return new Promise(resolve => {
        const image = new Image()
        image.onload = () => {
          const canvas = document.getElementById("myCanvas")
          const ctx = canvas.getContext("2d")
          ctx.clearRect(0, 0, canvas.width, canvas.height)
          canvas.width = image.width
          canvas.height = image.height
          ctx.drawImage(image, 0, 0, image.width, image.height)
          const base64 = canvas.toDataURL()
          resolve(base64)
        }
        image.src = src
      })
    }

  </script>
</body>
</html>
```
