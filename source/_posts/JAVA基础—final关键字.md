---
title: JAVA基础—final关键字
tags: JAVA基础
toc: true
---

[来自这里](深入理解Java中的final关键字)
## 定义
final在Java中是一个保留的关键字，可以声明成员变量、方法、类以及本地变量。一旦你将引用声明作final，你将不能改变这个引用了，编译器会检查代码，如果你试图将变量再次初始化的话，编译器会报编译错误。

## 使用
final是一个保留关键字，作用域可以使成员变量、类变量、方法、类、方法参数。
### 变量
- 如果是基本数据类型，那么其数值一旦在初始化之后便不能更改；
- 如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象，但是可以更改对象的属性值；

##### 类的成员变量
类的成员变量用final修饰后，在使用之前必须进行初始化，且只能初始化一次。

`private final int a ;`
```
    public FinalStudy(){
        a = 0;
    }
    public FinalStudy(int a){
        this.a = a;
    }
```
##### 类的类变量
类的类变量使用final修饰后，在定义时初始化或是在在静态块内进行初始化。
`public static final int t = 1;`
```
    public static final int t;
    static{
        t = 1;
    }
```
##### 方法的参数变量
如果作为方法的参数变量的修饰符，那么该值在方法内，不可以重新赋值。
```
    public void test(final int a){
        
    }
```

##### 方法的局部变量
如果作为方法的局部变量的修饰符，则该变量只能被定义一次。
```
    public void test(){
        final Integer i = 1;
    }
```
### 方法修饰符
当final修饰方法是，表明该方法不可以被子类重写，但是不影响函数重载。
如果你认为一个方法的功能已经足够完整了，子类中不需要改变的话，你可以声明此方法为final。final方法比非final方法要快，因为在编译的时候已经静态绑定了，不需要在运行时再动态绑定。
```
    public final void test(){
    }
```
### 类修饰符
使用final来修饰的类叫作final类。final类通常功能是完整的，它们不能被继承。
final与abstract是互斥的。
Java中有许多类是final的，譬如String, Interger以及其他包装类。
```
public final class Test{
}
```

## 好处
- final关键字提高了性能。JVM和Java应用都会缓存final变量。
- final变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销。
- 使用final关键字，JVM会对方法、变量及类进行优化。

## 常见面试题
### final修饰String拼接
```
        String a = "123";
        final String b = "12";
        String c = b + "3";
        System.out.println(a == c);//true
        String e = "12";
        String f = e + "3";
        System.out.println(a == f);//false
```
当final变量是基本数据类型以及String类型时，如果在编译期间能知道它的确切值，则编译器会把它当做编译期常量使用。
上述例子中，`b`为final对象，那么， `b+"3"`会在编译时进行拼接，为`"123"`，则指向常量区对应字符串的地址；
`e`为变量，`f`的值需要在运行时得到，运行时会在堆中创建新的对象；

### final修饰的引用变量指向的对象内容可变
final修饰的引用对象指向的对象不能改变，但是该对象的属性值可以修改。

## 总结
- final关键字可以用于成员变量、本地变量、方法以及类。
- final成员变量必须在声明的时候初始化或者在构造器中初始化，否则就会报编译错误。
- 你不能够对final变量再次赋值。
- 本地变量必须在声明时赋值。
- 在匿名类中所有变量都必须是final变量。
- final方法不能被重写。
- final类不能被继承。
- final关键字不同于finally关键字，后者用于异常处理。
- final关键字容易与finalize()方法搞混，后者是在Object类中定义的方法，是在垃圾回收之前被JVM调用的方法。
- 接口中声明的所有变量本身是final的。
- final和abstract这两个关键字是反相关的，final类就不可能是abstract的。
- final方法在编译阶段绑定，称为静态绑定(static binding)。
- 没有在声明时初始化final变量的称为空白final变量(blank final variable)，它们必须在构造器中初始化，或者调用this()初始化。不这么做的话，编译器会报错“final变量(变量名)需要进行初始化”。
- 将类、方法、变量声明为final能够提高性能，这样JVM就有机会进行估计，然后优化。
- 按照Java代码惯例，final变量就是常量，而且通常常量名要大写：