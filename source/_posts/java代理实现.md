---
title:  JAVA 代理模式
tags: [JAVA,代理]
toc: true
---
## 背景
代理，Proxy，在程序开发中有非常重要的作用。在Spring中，使用广泛的AOP，就是利用动态代理实现的。此为，Mybatis中，mapper类文件，也是通过动态代理，
生成MybatisFactoryBean。

### 静态代理
静态代理，原理上是：
- 借口I，它的实现类Impl(实现类是要被代理的类)
- 我们想要在Impl中的某个方法action()调用（或是全部方法调用）前（或后），添加一些其他的方法doBefor()/doAfter()，这时我们
需要有代理类ProxyImpl；
- 代理类需要实现接口I，同时，代理类的成员变量里，有Impl类的变量impl；
- 重写ProxyImpl类中的action()方法，调用doBefore()，然后调用impl的action，然后调用impl的doAfter()方法；
- 实际使用中，实例化ProxyImpl变量proxyImpl，调用proxyImpl的action()方法，即实现了代理的调用。

静态代理的一个问题就是，一般必须对每个实现类都生成一个代理类，才能达到我们需要的效果。

### 动态代理

动态代理的机制是，在程序运行时，利用反射生成动态代理的字节码，spring中AOP的实现，都是在spring初始化的时候，生成代理对象。

#### JDK动态代理

##### 实现
在JDK提供里，java.lang.reflect 包中的Proxy类和InvocationHandler 接口提供了生成动态代理类的能力。

- 接口Hello.java
```
package com.leegebe.proxy;

/**
 * @author  leegebe
 *
 */
public interface Hello {

    void sayHello();

}
```

- 实现类
```
package com.leegebe.proxy;

/**
 * @author  leegebe
 *
 */
public class HelloImpl implements Hello {

    public void sayHello() {
        System.out.println("一个普通的实现类");
    }

}
```

- 代理类
代理类需要实现`InvocationHandler.java`接口，然后调用Proxy.newProxyInstance方法，其中，参数为
- ClasserLoader
- 实现类的所有接口类
- 动态代理对象
```
package com.leegebe.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * @author leegebe
 * JDK的动态代理
 */
public class JdkProxy implements InvocationHandler {

    private Object target;

    public JdkProxy(Object target){
        this.target = target;
    }

    public void before(){
        System.out.println("before方法");
    }

    public void after(){
        System.out.println("after方法");
    }



    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        before();
        Object result = method.invoke(target,args);
        after();
        return result;
    }

    public static void main(String[] args){
        System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
        Hello hello = new HelloImpl();
        JdkProxy jdkProxy = new JdkProxy(hello);
        Hello proxyed = (Hello) Proxy.newProxyInstance(hello.getClass().getClassLoader(),hello.getClass().getInterfaces(),jdkProxy);
        proxyed.sayHello();
    }
}

```

##### 原理
JDK动态代理，我们可以看到其调用过程。
jdkProxy.main()->Proxy.newProxyInstance()->Proxy.getProxyClass0()->WeakCache.get()->
Factory.get()->Proxy.apply()->ProxyGenerator.generateProxyClass()