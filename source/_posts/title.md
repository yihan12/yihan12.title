---
title: 普通下载  &&  Vue文件图片下载处理
date: 2019-08-07 16:55:21
tags: [js,下载]
---

一般的下载,也就a标签加个链接地址,标签内加个download属性.

当地址是后端提供时:可通过创建a标签,
随即给a便签附下载链接,文件名和属性,
最后再创建点击效果,最后清楚生成的a标签.

再则是图片地址提供:可以通过Base64加canvas,对图片的下载可以进行处理.

下面就是相关方法处理函数

### HTML与文件下载

``` bash
<a href="large.jpg" download>下载</a>
```

### 文件下载配合后端表格导出
<!-- more -->
``` bash
export function downloadFile(url, filename) {
    // 创建隐藏的可下载链接
    var link = document.createElement('a');
    link.href = url;
    link.download = filename;
    link.target = '_blank';
    link.style.display = 'none';
    document.body.appendChild(link);
    // 触发点击
    link.click();
      // 然后移除
    document.body.removeChild(link);
    link = null;
}
```


加参数:
``` bash
export function generateQS(baseurl, paramObj) {
    var returnUrl = baseurl + '?'
    for (const key in paramObj) {
    // Object.hasOwnProperty(prop)用来判断对象是否含有指定属性
    // 返回值boolean
        if (paramObj.hasOwnProperty(key)) {
            const element = paramObj[key];
            returnUrl += key + '=' + element + '&';
        }
    }
    return returnUrl;
}


```

### 借助HTML5 Blob实现文本信息文件下载

``` bash
var funDownload = function (content, filename) {
    // 创建隐藏的可下载链接
    var eleLink = document.createElement('a');
    eleLink.download = filename;
    eleLink.style.display = 'none';
    // 字符内容转变成blob地址
    var blob = new Blob([content]);
    eleLink.href = URL.createObjectURL(blob);
    // 触发点击
    document.body.appendChild(eleLink);
    eleLink.click();
    // 然后移除
    document.body.removeChild(eleLink);
};
```

### 借助Base64实现任意文件下载

``` bash
var funDownload = function (domImg, filename) {
    // 创建隐藏的可下载链接
    var eleLink = document.createElement('a');
    eleLink.download = filename;
    eleLink.style.display = 'none';
    // 图片转base64地址
    var canvas = document.createElement('canvas');
    var context = canvas.getContext('2d');
    var width = domImg.naturalWidth;
    var height = domImg.naturalHeight;
    context.drawImage(domImg, 0, 0);
    // 如果是PNG图片，则canvas.toDataURL('image/png')
    eleLink.href = canvas.toDataURL('image/jpeg');
    // 触发点击
    document.body.appendChild(eleLink);
    eleLink.click();
    // 然后移除
    document.body.removeChild(eleLink);
};
```

### 结束语

在Chrome浏览器下，模拟点击创建的a元素即使不append到页面中，也是可以触发下载的，但是在Firefox浏览器中却不行，因此，上面的funDownload()方法有一个appendChild和removeChild的处理，就是为了兼容Firefox浏览器。