---
title: AAAAAlgorithm
date: 2018-03-28 17:44:53
tags:
---

## 记录一些工作中遇到的算法

##### 1. 一个树状结构被铺平后，通过 `parentId` 还原
```javascript
const getSkechLayers = (layers, art) => {
	const { artboard: { width, height, top, left, z } } = this.props
	const tree = {
	  width,
	  left,
	  top,
	  height,
	  z,
	  childrenLayers: []
	}
	const mappedData = {}
	layers.forEach(layer => {
	  mappedData[layer.id] = {
	    ...layer,
	    childrenLayers: []
	  }
	})

	for (var id in mappedData) {
	  const mappedLayer = mappedData[id]
	  if (mappedLayer.parent) {
	    mappedData[mappedLayer.parent]['childrenLayers'].push(mappedLayer)
	  } else {
	    tree.childrenLayers.push(mappedLayer)
	  }
	}
	return tree
 }
```

Time complexity: O(n)

##### 2. 判断两个矩形是否相交
```javascript
function isOverlap (a, b) {
  let [horMin, horMax] = a.left < b.left ? [a, b] : [b, a]
  let [verMin, verMax] = a.top < b.top ? [a, b] : [b, a]
  let isHorCross = horMax.left - horMin.left < horMin.width
  let isVerCross = verMax.top - verMin.top < verMin.height
  return isHorCross && isVerCross
}
```

### 3. 将数组中的几个元素后移或者前移一位
```javascript
/**
 * 将数组 array 中的某几个元素 itemsToMove 右移
 * 如移动 [a,b,c,d,e] 中的 [a,b] 得到 [c,a,b,d,e]
 * 1. 遍历数组, 当前元素需要移动时, 将其放入buffer, 继续遍历
 * 2. 当前元素不需要移动时, 将其放入新数组, 然后将buffer中的元素依次push到新数组, 清空buffer
 * 3. 遍历结束后, 将buffer中的剩余元素push到新数组
 */
const arrayItemsMoveRight = (array, itemsToMove) => {
  const newArray = []
  let buffer = []
  array.forEach(item => {
    if (itemsToMove.includes(item)) {
      buffer.push(item)
    } else {
      newArray.push(item)
      newArray.push(...buffer)
      buffer = []
    }
  })
  newArray.push(...buffer)
  return newArray
}

/**
 * 将数组 array 中的某几个元素 itemsToMove 左移
 * => 转换为对 reverse后的数组 的右移操作
 */
const arrayItemsMoveLeft = (array, itemsToMove) => {
  const reverseArray = [...array].reverse()
  return arrayItemsMoveRight(reverseArray, itemsToMove).reverse()
}
```
