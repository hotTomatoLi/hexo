---
title: Runtime.getRuntime()中freeMemory()|totalMemory()|maxMemory()
tags: [freeMemory,totalMemory,maxMemory]
toc: true
---

## 背景
在查看一个stactoverflowError时，看到`Runtime.getRuntime().freeMemory()`、
`Runtime.getRuntime().totalMemory()`、`Runtime.getRuntime().maxMemory()`等内容，在此记录一下。

## 介绍
三个方法都是本地方法。
### freeMemory
JDK中
```
    /**
     * Returns the amount of free memory in the Java Virtual Machine.
     * Calling the
     * <code>gc</code> method may result in increasing the value returned
     * by <code>freeMemory.</code>
     *
     * @return  an approximation to the total amount of memory currently
     *          available for future allocated objects, measured in bytes.
     */
    public native long freeMemory();
```
是个native方法，说明如下：
返回了Java虚拟机中的空余(free)内存的数量。
调用gc的方法，也许会增大freeMemory的值。
在返回值中说明了，该值是一个估计(approximation)值，单位为字节(bytes)，该数值表明虚拟机在将来分配对象空间时可用的空间大小。


### totalMemory
```
    /**
     * Returns the total amount of memory in the Java virtual machine.
     * The value returned by this method may vary over time, depending on
     * the host environment.
     * <p>
     * Note that the amount of memory required to hold an object of any
     * given type may be implementation-dependent.
     *
     * @return  the total amount of memory currently available for current
     *          and future objects, measured in bytes.
     */
    public native long totalMemory();
```
返回了java虚拟机中的内存总大小。这个方法的返回值也许会超时（在不同的环境下），即当前查看的值和实际的值不一致。

任意给定的类型，需要的内存空间，依赖于具体的实现，不同的虚拟机也许不一致。

返回值说明中，该值单位为字节，该值表明了当前对现在和未来的对象可以使用的内存空间。


### maxMemory

```
  /**
     * Returns the maximum amount of memory that the Java virtual machine will
     * attempt to use.  If there is no inherent limit then the value {@link
     * java.lang.Long#MAX_VALUE} will be returned.
     *
     * @return  the maximum amount of memory that the virtual machine will
     *          attempt to use, measured in bytes
     * @since 1.4
     */
    public native long maxMemory();
```
返回了虚拟机可以使用的最大内存。如果没有限制，将会返回java.lang.Long#MAX_VALUE。

返回值说明中，该值单位为字节，是虚拟机能够使用的最大内存。


## 测试
启动一个程序，当不设置任何配置时，
初始进入时(单位kB)
max     1844224
total    124416
free     121753   

再进入
max     1844224
total    124416
free     96751 

再进入
max     1844224
total    124416
free     96085 

再进入
max     1844224
total    124416
free     85922


 当进行配置
 ```
 -server -Xms256m -Xmx512m -Xmn128m -XX:PermSize=128m -Xss160k -XX:+DisableExplicitGC -XX:+UseConcMarkSweepGC -XX:+CMSParallelRemarkEnabled -XX:+UseCMSCompactAtFullCollection -XX:LargePageSizeInBytes=128m -XX:+UseFastAccessorMethods -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=70
 ```
 初始进入时
 max     511232
total    249088
free     242790

再进入
max     511232
total    249088
free     142084

 再进入
max     511232
total    249088
free     138054

修改参数`-Xmx768m`
 初始进入时
 max     773376
total    249088
free     242790

 再进入
max     773376
total    249088
free     142145

 再进入
max     773376
total    249088
free     138114

修改参数`-Xms300m`
 初始进入时
 max     773376
total    300288
free     293990

 再进入
max     773376
total    300288
free     193274

 再进入
max     773376
total    300288
free     189243

### 总结
`maxMemory`与配置`-Xmx`一致，在初始时，`totalMemory`与`-Xms`有关系，初始值与`-Xms`值一致。
而`total-free`为当前使用的内存大小。