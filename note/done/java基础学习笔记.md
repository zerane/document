# 前言 #
java以前学的比较水，温故而知新。但是太基础的东西就不写了。

## 程序设计基础 ##

算式|结果
--|--
(int)1185/(int)100|11
(int)1185/(float)100|11.85

java原来也有变长参数
> void func(double...numbers);

里面和数组一样用

## 面向对象程序设计 ##

- String：不可变类
- StringBuffer：可变类，线程安全
- StringBuilder：可变类，线程不安全，效率比StringBuffer高

父类的构造函数可以直接用super()调

(new LinkedList<>() instanceof LinkedList)==true，这个可以用在覆盖.equals()中

final类不能作为父类，final方法不会被覆盖会直接报错

finalize是析构函数，不过java不用手动调，这函数就是个大坑，无法手动控制什么时候调，释放资源还是自己写吧。

List<> ans = new LinkedList<>();

- 数据域：list的数据域
- 方法：linkedlist的方法，其方法调用的是linkedlist的数据域
- 父类方法调用实例方法域，但是调用父类数据域，但个人感觉还是数据域不重叠好

接口可以看作是一个只有抽象函数的类，实现接口要讲所有抽象函数覆盖，这样实例强转为接口就可以直接调用对应实现的抽象函数，从而在强转中不发生继承问题，从而一定程度上可以看作多继承


## 图形界面 ##

现在貌似没人用？先略

## 异常处理、io、递归 ##

这章节名不忍吐槽- -

一张异常分类图：
> ![](http://i.imgur.com/yuOApAp.jpg)

trycatch后会继续运行，但是如果catch完又throw出去那么程序会从throw处跳出

assert挺方便，但是需要在java的运行参数里加-ea，还有catch抓不到assert丢出来的异常

io就是几个输入输出流，还有一个randomaccessfile，记得关，丢异常，还有scanner要关(这个老是忘)

