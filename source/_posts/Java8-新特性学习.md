---
title: Java8 新特性概览
date: 2016-06-29 10:06:29
tags: Java8
---

Java8  的新特性可以用一本书来描述，这里是笔者通过简单学习后的一些记录，主要涉及到 Java8  的默认接口方法、函数式接口、Lambda 表达式、java 8 新的日期API 和 Java8  新增加的工具类。

<!--more-->

## Java8 接口默认方法

引入原因：默认方法允许您添加新的功能到现有库的接口中，并能确保与采用旧版本接口编写的代码的二进制兼容性。

具体实例：
forEach 方法是 jdk 1.8 新增的接口默认方法，正是因为有了默认方法的引入，才不会因为 Iterable 接口中添加了 forEach 方法就需要修改所有 Iterable 接口的实现类，而且forEach 使用了内循环，相对之前的for循环，有以下优点：
1、 不一定需要顺序处理List中的元素，顺序可以不确定
2、可以并行处理，充分利用多核CPU的优势
3、 有利于JIT编译器对代码进行优化

除了默认方法，Java8  还在允许在接口中定义静态方法。

## Java8 函数式接口

一个接口，如果只有一个显式声明的抽象方法，那么它就是一个函数接口。一般用@FunctionalInterface标注出来（也可以不标）。举例如下：

    @FunctionalInterface
    public interface Runnable { void run(); }
    public interface Callable<V> { V call() throws Exception; }
    public interface ActionListener { void actionPerformed(ActionEvent e); }
    public interface Comparator<T> { int compare(T o1, T o2); boolean equals(Object obj); }

Java8 中新加了个java.util.function包，里面添加了Function, Supplier, Consumer, Predicate等函数式接口，这些接口经常和 Stream 结合着用:

* Function<K,T>  K 是输入类型，T是输出类型，其实就是一个函数的封装
* Predicate<T> -T作为输入，返回的boolean值作为输出，常用作filter
* Consumer<T> - T作为输入，执行某种动作但没有返回值


## Java8  之 Lambda 表达式

java是一门面向对象的语言，在Java8之前，它也是声明式编程语言，而 Java8 中 Lambda表达式的引入，使得它在写法上具有了函数式编程的特点。但它仍然是面向对象的，因为在Java8中的编写 Lambda 表达式，实质上定义了一个匿名方法，而这个方法的后面，至少有声明一个函数接口。

关于 Lambda 表达式，推荐书籍《精通lambda表达式：Java多核编程》

另外有了 Lambda 表达式，我们可以[不要在使用循环了](http://www.importnew.com/14841.html)

## Lambda  和 Stream 

Java8  中的 Stream是对集合（Collection）对象功能的增强，它专注于对集合对象进行各种非常便利、高效的聚合操作（aggregate operation），或者大批量数据操作 (bulk data operation)。Lambda 表达式也是一种语法糖，在编写上给我提供了方便。Java8 中引入的 Strema，则紧密结合了 Lambda 表达式，充分的发挥了它的便捷性。我们可能在学习 Stream 的同时熟悉 Lambda 表达式。

Stream 表示能应用在一组元素上一次执行的操作序列。Stream 操作分为中间操作或者最终操作两种，最终操作返回一特定类型的计算结果，而中间操作返回Stream本身，这样我们就可以将多个操作依次串起来。

关于 Stream 的学习，推荐IBM 的[Java8  中的 Streams API 详解](http://www.ibm.com/developerworks/cn/java/j-lo-Java8streamapi/)。

## 日期时间API

Java8 使用了 **JSR 310** 规范，新增了java.time包。
在Java8 版之前，如果我们想格式化日期，必须使用SimpleDateFormat类，用它格式化输入的日期类。而Java8 引入了以下的新日期时间类：
LocalTime、LocalDate 、LocalDateTime、OffsetDate、OffsetTime、OffsetDateTime

## 工具类

*  并行数组排序
Java8 在java.util.Arrays类中新增了并行排序功能，能够更充分地利用多线程机制。

```
import java.util.Arrays;
public class ParallelSort {
    public static void main(String[] args) {
        int arr[] = {1, 4, 2, 8, 5};
        Arrays.parallelSort(arr);
        for(int i : arr){
            System.out.print(i + " ");
        }
    }
}
```

*  java.util.Optional类
Java8 在java.util包中新增了Optional类，Optional 类是一个可以包含或不可以包含非空值的容器对象。主要用在判断对象空指针，提供了 orElseThrow、orElse 和 orElseGet 等方法。Optional 类更像是一个容器，它保存一个类型的值或是null值。通过使用 Optional 类的 isPresent() 方法，我们可以检查指定的对象是否为空。更多方法参考 [Java8  Optional类深度解析](http://www.importnew.com/6675.html)



