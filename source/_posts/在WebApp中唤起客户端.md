---
title: 在WebApp中唤起客户端
date: 2018-01-26 12:06:06
tags:
  [IOS, javascript, Android]
---
当你在浏览器中需要打开或者下载某个应用时，往往你希望直接点击相应按钮即可打开或去应用商店下载该应用，
而对于Android和IOS用户，唤起应用的操作是不同的，下面我将讲到如何唤起或打开应用商店下载相应应用。

### 1. 设置应用商店地址

首先设置好跳转至应用商店的URL

```javascript
// .js 文件中
const ios_client_url = "https://itunes.apple.com/appdir";
const android_client_url = "https://play.google.com/store/apps/details?id=xxx";

// 假设你有一个函数来判断当前设备是否为Android设备，isAndroid()
const downloadURL = isAndroid() ? android_client_url : ios_client_url;
```
在HTML文件中创建一个button标签
```html
<!-- .html文件 -->
<button id="btn" onClick="openInClient()">在客户端打开</button>
```

### 2. 通过按钮打开客户端

要打开客户端，首先需要确认设备上装有客户端，若设备上没有，则跳转至应用商店下载
参考[唤起IOSApp](https://stackoverflow.com/questions/13044805/how-to-check-if-an-app-is-installed-from-a-web-page-on-an-iphone)

```javascript
// .js文件
function openInClient() {
  // 若设备上没有安装客户端，无法唤起客户端，该语句会执行，跳转至应用商店
  setTimeout(() => { window.location = downloadURL }, 25);

  // 判断设备为Android设备 还是IOS设备 并唤起客户端
  if (isAndroid()) {
    // intent-based URI语法
    window.location = `intent://my_host#Intent;package=my_package;scheme=my_scheme;action=my_action;component=my_component;S.browser_fallback_url=${downloadURL};end`;
  } else {
    // appname为IOS客户端的协议名
    window.location = "appname://";
  }
}
```
关于[唤起安卓App](https://stackoverflow.com/questions/11773958/open-android-application-from-a-web-page)
若你想了解更详细的唤起安卓App的技术，请看[这里](https://jaq.alibaba.com/community/art/show?articleid=265)
