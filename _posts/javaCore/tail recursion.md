---
layout: default
title: 尾递归
---

最简单的一个解释，函数最后一步调用自己，没有任何其他计算，包括 `1+ return` 这样的也不行
盗个知乎答案
> 这是尾递归
```
function f(x) {
   if (x === 1) return 1;
   return f(x-1);
}
```
> 这不是尾递归
```
function f(x) {
   if (x === 1) return 1;
   return 1 + f(x-1);
}
```

两段代码的差别就是返回时，非尾递归带了 `1+` 

[知乎原文地址](https://www.zhihu.com/question/20761771)
