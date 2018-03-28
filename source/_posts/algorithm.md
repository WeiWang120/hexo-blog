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