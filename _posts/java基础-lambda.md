title: Java基础(lambda)
tags: java se
categories: java
feature: img/java_feature.jpg
description: Java8新特性是Lambda表达式.
comments: true
recommend: true
declaration: true
toc: true
date: 2015-11-16 14:59:29
---

### 简介
Lambda表达式是Java SE 8 非常重要的新特性。对于单方法的接口，Lambda表达式提供一种干净简洁的代码实现方式。同时，Lambda表达式改良了`Collection`类包，使得迭代，filter和extract data from a Collection更简单。并且，其并行的特性在多核环境中性能得到很大提高。

<!--more-->
### 使用

#### 语法
`(parameters) -> {expression body}`
说明：
1. 参数类型声明是可选的，编译器可以根据参数的值推断出来。
2. 只有一个参数时，参数两边的括号可以省略。单多个参数时还是必须要用括号括起来的
3. 当`expression body`中只有一个语句时,外面的花括号可以省略
4. 当`expression body`只有一个表达式时，并且该表达式返回一个值，该值就是Lambda的需要返回值时，可以省略`return`语句，但是这种情况下不能省略花括号。

#### 示例
```java
/**
 * Test for Lambda
 */
package com.kalven.lambda.mytest;

/**
 * @author kalven.meng
 *
 */
public class LambdaTest {
    
    private String name = "Poly";
    /**
     * @param args
     */
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        new LambdaTest().rename((test,newname) -> {
            String oldName = test.getName();
            test.setName(newname);
            return oldName;
        });
    }

    /**
     * rename 
     * @param person
     */
    public void rename( Person person) {
        // TODO Auto-generated method stub
        System.out.println("old name:" + person.rename(this, "Jhon"));
    }
    /**
     * @return the name
     */
    public String getName() {
        return name;
    }
    /**
     * @param name the name to set
     */
    public void setName(String name) {
        this.name = name;
    }
}
/**
 * for test
 * 
 * @author kalven.meng
 *
 */
interface Person{
    /**
     * set name to newname and return the old name
     * @param test TestObj
     * @param newName new name
     * @return old name
     */
    String rename(LambdaTest test, String newName);
}

```
#### ` java.util.function`包
该包提供了很大`functional interface`的标准接口
1. Predicate：对参数进行断言；接收T对象并返回boolean
2. Consumer：在给定参数上执行某操作；接收T对象，不返回值
3. Function：接受一个参数并产生结果的函数；接收T对象，返回R对象
4. Supplier：返回一个T的实例（如工厂）；提供T对象（例如工厂），不接收值
5. UnaryOperator：表示在同一类型的操作数的操作后，产生相同类型的结果作为操作数；接收T对象，返回T对象
6. BinaryOperator：A binary operator from (T, T) -> T；接收两个T对象，返回T对象

#### 对Collection 类的改进
先提供一个测试用的类Person.java：
```java
/**
 * 
 */
package com.kalven.lambda.mytest;

import java.util.ArrayList;
import java.util.List;

/**
 * @author kalven.meng
 *
 */
public class Person {
    private String name;
    private int age;
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    /**
     * @return the name
     */
    public String getName() {
        return name;
    }
    /**
     * @param name the name to set
     */
    public void setName(String name) {
        this.name = name;
    }
    /**
     * @return the age
     */
    public int getAge() {
        return age;
    }
    /**
     * @param age the age to set
     */
    public void setAge(int age) {
        this.age = age;
    }
    /**
     * 生成测试list
     * @return
     */
    public static List<Person> createShorPersonList(){
        List<Person> persons = new ArrayList<Person>();
        persons.add(new Person("Lily", 20));
        persons.add(new Person("Lucy", 24));
        persons.add(new Person("Jack", 28));
        persons.add(new Person("Jhon", 12));
        return persons;
    }
    
    /**
     * 获取person信息
     * @return
     */
    public String getInfo(){
        StringBuilder info = new StringBuilder();
        info.append("Name:").append(this.getName()).append(",age:").append(this.getAge());
        return info.toString();
    }
}

```
1.  `Looping`：`Collection`的遍历
    ```java
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List<Person> persons = Person.createShorPersonList();
        //遍历person，打印person信息
        persons.forEach(p -> System.out.println(p.getInfo()));
    }
    ```
2.  `Chaining and Filters`：`Collection`的链式操作和过滤
    ```java
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List<Person> persons = Person.createShorPersonList();
        //打印age在20和30之间的person
        persons.stream().filter(p -> p.getAge()>=20&&p.getAge()<=30)
        .forEach(p -> System.out.println(p.getInfo()));
    }
    ```
3.  `Mutation`和`Results`：过滤并生成新的`Collection`
    ```java
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List<Person> persons = Person.createShorPersonList();
        //过滤后生成新的list
        List<Person> personsNew = persons.stream().
                filter(p -> p.getAge()>=20&&p.getAge()<=30).collect(Collectors.toList());
        personsNew.forEach(p -> System.out.println(p.getInfo()));
    }
    ```
4.  使用`map`计算
    ```java
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List<Person> persons = Person.createShorPersonList();
        //计算总年龄和平均年龄
        int totalAge = persons.stream().
                filter(p -> p.getAge()>=20&&p.getAge()<=30)
                .mapToInt(p -> p.getAge()).sum();
        OptionalDouble averageAge = persons.stream().
                filter(p -> p.getAge()>=20&&p.getAge()<=30)
                .mapToInt(p -> p.getAge()).average();
        System.out.println("totalAge:" + totalAge + " ,averageAge:" + averageAge.getAsDouble());
    }
    ```

### 扩展
参考：
1.  [Java SE 8: Lambda Quick Start](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Lambda-QuickStart/index.html "Lambda 表达式入门教程")
2.  [Java 8 - Lambda Expressions](http://www.tutorialspoint.com/java8/java8_lambda_expressions.htm "Lambda表达式教程")
