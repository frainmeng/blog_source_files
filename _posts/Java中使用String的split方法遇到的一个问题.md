title: Java中使用String的split方法遇到的一个问题
tags: java se
categories: java
feature: img/java_feature.jpg
description: split方法切割使用`|`分隔的字符串.
comments: true
recommend: true
declaration: true
toc: true
date: 2015-11-20 09:32:11
---
### 问题
使用String的split方法分割下面字符串时，没有得到预期的值：
```java
public static void main(String[] args) {
    // TODO Auto-generated method stub
    String str = "20140613|1|8000000181|33330013||15615700100101233|9|1.00|1563.00|20140613|S1|";
    String[] strs = str.split("|");
    System.out.println(strs);
    
}
```
结果：
```
[2, 0, 1, 4, 0, 6, 1, 3, |, 1, |, 8, 0, 0, 0, 0, 0, 0, 1, 8, 1, |, 3, 3, 3, 3, 0, 0, 1, 3, |, |, 1, 5, 6, 1, 5, 7, 0, 0, 1, 0, 0, 1, 0, 1, 2, 3, 3, |, 9, |, 1, ., 0, 0, |, 1, 5, 6, 3, ., 0, 0, |, 2, 0, 1, 4, 0, 6, 1, 3, |, S, 1, |]
```
<!--more-->

### 解决方法
将`|`改为`\\|`,并且最好使用`split(String regex, int limit)`，limit取负值(参考api)时表示，不忽略尾部空值：
```java
public static void main(String[] args) {
    // TODO Auto-generated method stub
    String str = "20140613|1|8000000181|33330013||15615700100101233|9|1.00|1563.00|20140613|S1|";
    String[] strs = str.split("\\|",-2);
    System.out.println(strs);
    
}
```

### 总结
`split()`方法使用的是正则表达式，而`|`、`.`、`&`、`*`、`+`等是正则表达式中的特殊字符，需要使用`\\`进行转义