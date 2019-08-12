---
title: IOS微信分享问题
date: 2019-08-09 11:16:29
tags: [js-sdk,js,vue]
---

之前一个vue项目遇到的问题:微信分享在安卓上没问题,但是到了IOS就会分享报错;
主要问题是:Vue的push跳转,在IOS端会只跳转页面,地址栏的导航不会发生变化;
而微信分享的js-sdk必须签名地址和页面地址必须一致才能分享成功.
网上方法差不多都看了，有个解决方法：window.location.href,确实有效，但是必须进入页面后再次刷新页面才能签名成功,但是这样的话,会导致页面的所有数据会重新加载影响用户体验.


### 区分安卓和IOS端,并做出不同的处理

```bash
window.router=router;
router.afterEach(to => {
  const u = navigator.userAgent.toLowerCase();
  if (
    u.indexOf("like mac os x") < 0 ||
    u.match(/MicroMessenger/i) != "micromessenger"
  )
    return;
  if (to.path !== global.location.pathname) {
    location.assign(to.fullPath);
  }
});
```
<!--more-->
### 总结

亲测window.location.href是有用但是需要再次刷新页面才会签名成功，！window.location.href刚跳转进去是不能签名成功的；改变全局路由守卫后置钩子就不需要改变push的切换页面方式，当它是ios端的时候会主动改变的url。还有window.location.href有个跳转效果不好，还会重新获取数据.




