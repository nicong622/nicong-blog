---
title: Textarea 自适应高度
date: 2020-10-07
---

如何使 *textarea* 元素的高度可以根据内容自动伸缩？

只需两行代码：

```js
// 当输入的时候
textarea.style.height = 'auto';
textarea.style.height = textarea.scrollHeight + 'px';
```

第一行的作用是在删除输入的内容时候让 *textarea* 的高度能自动缩回去；

在 Vue 中使用：https://codepen.io/Nicong622/pen/LYZYNqb

