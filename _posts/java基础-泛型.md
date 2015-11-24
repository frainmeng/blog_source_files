title: java基础----泛型
tags: java se
categories: java
feature: img/java_feature.jpg
description: Java 泛型的用法.
comments: true
recommend: true
declaration: true
toc: true
date: 2015-11-12 14:51:56
---

在Java的类中`java.util.concurrent.ForkJoinTask`偶然看到这样一段代码
```java
public static <T> ForkJoinTask<T> adapt(Callable<? extends T> callable) {
        return new AdaptedCallable<T>(callable);
}
```
其中`<T> ForkJoinTask<T>`这个泛型让我无法理解，以前从没有遇见过这样使用泛型的，那就查吧。

<!--more-->

### 常见的几种泛型表示
因为不知道是不是还有其他的表示，所以这里就说他们是常见的吧
1.  `<T>,<K>,<V>,<E>`等
    这种用法就是在<>中指定泛型，其中的字母如T(type)、K(key)、V(value)、E(element)）等只是用一个标识符，你也可以用A、B、C等其他的字母代替
    如果有多个类型需要泛型的话就用一个`,`分隔开就行了比如我们常用的`Map<K,V>`
    例如:

    ```java
    public class Test <T> {
        T list;
    }
    ```

2.  `<?>`通配符
    这个就是简单的表示java中的任意类了
    例如：
    ```java
    List<?> list = new ArrayList<String>();//效果等同于List list = new ArrayList()
    ```
3.  限定范围
    限定可以分为限定上限和限定下限
    限定上限使用`extends`关键字：
    <? extends A> 接受A或者A的子类
    限定下限使用`supper`关键字：
    <? supper A> 接受A或者A父类

### 自定义泛型
泛型的定义我见到的是有三种种：泛型类、泛型接口和泛型方法
1.  泛型类
    将泛型应用于类
    例如：
    ```java
    public class TT <K,V> {
        private K key;
        private V value;
        public TT(K key,V value) {
            this.key = key;
            this.value = value;
        }
    }
    ```
    其中`K,V`的作用范围是整个类
2.  泛型接口
    将泛型应用于接口
    例如：
    ```java
    public interface Generator<T> {  
        T next();  
    }
    ```
    同样泛型`T`的作用范围是整个类
3.  泛型方法
    将泛型应用于方法
    例如：
    ```java
    public <E> E getEle(List<E> list) {
        return list.get(0);
    }
    ```
    这里可以看到在返回类型E 前面还有个`<E>`，这个的作用表示E这个泛型的作用范围限定在这个方法。如果是在泛型类内部定义泛型方法时要特别注意这点，这样使用可以防止于类中的泛型冲突。

### 补充
在使用泛型的时候，我们有时候可能想要实例化一个泛型的数组，向下面这种
```java
public class TT <T> {
    Class<T> type;
    private int length;
    public TT(int length) {
        this.length = length;
        T[] arrays = new T[length];//这里编译报错不能通过；
    }
}
```
针对这种情况可以使用下面这个方式来实现;
```java
public class TT <T> {
    Class<T> type;
    private int length;
    public TT(int length) {
        this.length = length;
        T[] arrays = (T[]) Array.newInstance(type, length);
    }
}
```
这里用到了`java.lang.reflect.Array`这个类，这个类是java的反射包的类，它支持动态类型，即动态生成数组。

### 扩展
如果想了解更多关于java的泛型，可以看看这个人的一篇博客，写的很详细：
[泛型 —— Java编程思想 ](http://jiangzhengjun.iteye.com/blog/540535)
