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

----

2月6日更新：

本来以为这个唤起客户端的功能就这样结束了，结果测试那边发现安卓机上 UC 浏览器无法唤起客户端，如果用 intent 直接跳404，网上搜索了各种资料都没有很好的解决掉这个问题，但是发现一般都会去用 `iframe.src` 来执行跳转功能。并且对于非 Chrome 的浏览器一般不会用 intent 去跳转，而是用协议名。于是我也进行了尝试

```javascript
  // .js file
  const openAndroidApp = (fallbackUrl) => {
  const { hostname, pathname } = location
  const userAgent = navigator.userAgent.toLowerCase()
  const iframe = document.createElement('iframe')
  document.body.appendChild(iframe)
  iframe.style.cssText='display:none;width=0;height=0'
  setTimeout(function () { window.location = fallbackUrl }, 200)
  if (
    userAgent.includes('chrome') &&
    userAgent.includes('android') &&
    !userAgent.includes('ucbrowser') &&
  ) { // Chrome 中
    const url = `intent://${hostname}${pathname}#Intent;package=my_package;scheme=my_scgene;S.browser_fallback_url=${fallbackUrl};end`
    iframe.src = url
  } else { // UC 浏览器
    const url = `my_scheme://${hostname}${pathname}?password=${encryptedPassword}`
    iframe.src = url
  }
}
```

这样写发现不跳404了，但是也无法唤起客户端，会直接跳转至应用商店。于是我下载了一个 UC 开发者版本，和 Chrome 连在一起看 console，发现原来 UC 浏览器在 https 协议下不会去跳转到一个非 https 的协议去，会报错 `net::ERR_UNKNOWN_URL_SCHEME`。于是我把在 UC 下的跳转从 `iframe.src = url` 改成了 `window.open(url)`。这下终于能跳转成功了！！！但是还是会有不完美的地方，那就是浏览器会自己新开一个 tab，这样当你回到浏览器时会看到一个空白的页面。 不过这已经是目前我能够解决这个跳转的问题的唯一方法了。。
