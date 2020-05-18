---
layout: default
title: removeHead的bug
---

下面C语言函数代码想要删除单链接的头节点，找出并修复其中所有的 bug
```
void removeHead( ListElement *head ){
    free( head );        // Line 1
    head = head->next;   // Line 2
}
```
> Excerpt From: John Mongan. “Programming Interviews Exposed.” iBooks. 

对于你得到的任何函数，考虑下面四个常见的问题

## Check that the data comes into the function properly
## 检查进入函数的数据的正确性


## Check that each line of the function works correctly
## 检查函数的每一行，确保运行正确

## Check that the data comes out of the function correctly
## 检查函数的输出，确保正确

## Check for common error conditions. 
## 检查常见的错误条件


=
了解更多请关注我的公众号
![](http://note.youdao.com/yws/api/personal/file/D4F33BFCDB0846F09597A867DE622CF9?method=download&shareKey=c7d1dd0af3045032bcd2f61952494356)