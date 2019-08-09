---
layout: post
title: markdown基本语法
category: 工具
description: markdown
---

# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题

**这是加粗的文字**
*这是倾斜的文字*
***这是斜体加粗的文字***
~~这是加删除线的文字~~

>这是引用的内容
>>这是引用的内容
>>>>>>>>>>这是引用的内容

## 分割线

---
----
***
*****

## 图片
![blockchain](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/
u=702257389,1274025419&fm=27&gp=0.jpg "区块链")

## 超链接
[简书](http://jianshu.com)
[百度](http://baidu.com)

## 列表
**无序列表**
- 列表内容
+ 列表内容
* 列表内容

注意：- + * 跟内容之间都要有一个空格

**有序列表**

1.列表内容
2.列表内容
3.列表内容

注意：序号跟内容之间要有空格

**列表嵌套**

上一级和下一级之间敲三个空格即可

- 一级列表
   - 二级列表
  
## 表格

姓名|技能|排行
--|:--:|--:
刘备|哭|大哥
关羽|打|二哥
张飞|骂|三弟

## 代码

```
    function fun(){
         echo "这是一句非常牛逼的代码";
    }
    fun();
```

## 流程图

```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
&```