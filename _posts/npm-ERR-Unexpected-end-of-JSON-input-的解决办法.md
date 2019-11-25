---
title: npm ERR! Unexpected end of JSON input 的解决办法
tags:
  - deug
  - npm
date: 2018-09-19 18:18:00
---


运行 `npm install` 时遇到下面的提示

`npm ERR! Unexpected end of JSON input while parsing near 'popper.js'`

然而查看 package.json 并没有找到上面的字段，查阅 [stackoverflow](https://stackoverflow.com/questions/47675478/npm-install-errorunexpected-end-of-json-input-while-parsing-near-nt-webpack) 后，得到以下解决办法：

```bash
npm cache clean --force
```
