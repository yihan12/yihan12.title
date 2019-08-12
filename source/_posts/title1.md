---
title: js判断文件名是否合法
date: 2019-08-07 17:25:37
tags: [js,type]
---

文件类型可查询MIME参考手册.

### 获取文件后缀名

``` bash
/**
 * @description 获取文件后缀名
 * @param {String} fileName 文件全名，包含后缀名的那种
 */
export function getFileExt(fileName) {
    var splits = fileName.split('.');
    return _.last(splits);
}
```

### 检查文件类型

<!-- more -->
``` bash
/**
 * @description 检查文件类型，是否是合法的，这里的validMIMEList仅写了部分，如果需要支持更多，请查询MIME参考手册，增加更多的MIME类型进来
 * @param {Object} file 文件对象
 * @param {String} exts 文件合法类型，格式：doc|docx|png
 */
export function checkFileType(file, exts) {
    var validMIMEList = [
        // doc
        'application/msword',
        // xls
        'application/vnd.ms-excel',
        // docx
        'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
        // xlsx
        'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
        // pdf
        'application/pdf',
        // rar
        'application/x-rar-compressed',
        // zip
        'application/zip'
    ];
    var validExts = exts.split('|');
    var fileExt = getFileExt(file.name);
    if (_.includes(validMIMEList, file.type) || _.includes(validExts, fileExt)) {
            return true;
    } else {
            return false;
    }
}
```