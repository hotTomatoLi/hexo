---
title: JAVA基础-HashMap
tags: JAVA基础
toc: true
---

## 用途

## 名词说明
|名词|描述|
|:--:|:--:|
|bucket|桶|
|bin|箱子|
## 源码结构
### 常量

![常量](http://upload-images.jianshu.io/upload_images/1213316-3d2e5ebc515bb001.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

|名称|用途|
|:--:|:--:|
|serialVersionUID|序列化ID|
|DEFAULT_INITIAL_CAPACITY|默认容量，16|
|MAXIMUM_CAPACITY|最大容量，2的30次方|
|DEFAULT_LOAD_FACTOR|默认加载因子0.75，加载因子用于扩容时，如果当前已经存储的数据量大于总容量的0.75，会进行扩容|
|TREEIFY_THRESHOLD|8，这个阈值表明，当一个bucket有超过8个节点时，会将链表转成树|
|UNTREEIFY_THRESHOLD|6，这个阈值表明，当一个bucket小于6个节点时，会将链表转成树|
|MIN_TREEIFY_CAPACITY|bin被树化时最小的hash表容量|

### 变量
- transient ：java语言的关键字，变量修饰符，如果用transient声明一个实例变量，当对象存储时，它的值不需要维持。换句话来说就是，用transient关键字标记的成员变量不参与序列化过程。

![变量](http://upload-images.jianshu.io/upload_images/1213316-4d3effbbd092efc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


|名称|数据类型|描述|
|:--:|:---:|:--:|
|table|Node<K,V>[]|HashMap的实际数据结构，为一个数组，Node<K,V>为内部定义的数据结构。|
|entrySet|Set<Map.Entry<K,V>>| ---|
|size|int| 当前存储数据的数量|
|modCount|int| 记录HashMap结构修改的次数|
|threshold|int| 触发扩容的数值|
|loadFactor|int| 加载因子，加载因子用于扩容时，如果当前已经存储的数据量/总容量>loadFactor，会进行扩容|

### 内部类
#### Node
Node类是一个HashMap中存储的基本数据（）
- hash 哈希值，在table[i]位置存储的Node初始是队列，队列的所有的hash值相等
- key 主键
- value 值
- next 指向链表的下个Node

#### KeySet
存储所有Key值的Set，Key值不重复。

#### Values
存储所有的Value类

#### EntrySet

#### HashIterator
抽象类

#### KeyIterator

#### ValueIterator

#### EntryIterator

#### HashMapSpliterator

#### KeySpliterator

#### ValueSpliterator

#### EntrySpliterator

#### TreeNode

### 函数
#### 构造函数
HashMap提供四种构造函数。
- HashMap(int initialCapacity, float loadFactor)，指定初始容量和加载因子
- HashMap(int initialCapacity)，指定初始容量
- HashMap()，采用默认值
- HashMap(Map<? extends K, ? extends V> m)，根据一个Map初始化hashMap

#### hash(Object key)
```
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```
至于为什么会有移位异或，可以参看[这里](https://www.zhihu.com/question/20733617)。
总结如下：
- 代码的学名叫"扰动函数"
- int类型32位，右移16位，再将高地位异或，混合原始哈希码的高位和低位，以此来加大低位的随机性


#### comparableClassFor(Object x)
判断一个对象是否实现了`Comparable`接口，如果是，返回x的class对象；否，返回null。

#### compareComparables(Class<?> kc, Object k, Object x)
判断对象x的class类别不是是kc，返回0；否则，比对k和x的大小。

#### tableSizeFor(int cap)
返回大于等于cap的最小的2的整幂数，如果超过最大值，则返回最大值。
解释可以参看[这里](http://blog.csdn.net/fan2012huan/article/details/51097331)
```
 static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= (1 << 30)) ? (1 << 30) : n + 1;
    }
```
通过将n不断移位或运算（加上n的二进制为1的最大位为Bit），得到Bit之后所有位值都是1的数，然后加1，得到最小的2的整幂数。
`cap-1`的原理如下：
假设一个数值，二进制有m个1，如果
- m=1，则m-1的最高位会降低，即最高位会变成原来位后一位；
- m>1，m-1的最高位不会降低；

只有1个1，要借位到最高位；多个1，不用借位到最高，最高位不变。

最终目的是为了得到大于等于cap的最小的2的整幂数，所以，通过`cap-1`，假设原来是`0000 1000`，减一得到`0000 01111`，最终再加1，得到本身；如果原来的不是2的整幂，减一也不会降低最高位，那么最终结果就是大于该值的最小2的整幂数。

#### putMapEntries(Map<? extends K, ? extends V> m, boolean evict)

#### size()
返回大小。

#### isEmpty()
判断是否为空。

#### get(Object key)
获取对象。

####  getNode(int hash, Object key)
- 根据给定的hash值，计算在`table`中的位置((n - 1) & hash，n为table的size),得到first
- 如果该位置的第一个节点first（Node<K,V>）的`key`值与输入的`key`一致，则返回first；
- 否则，遍历first对应的列表或是tree，得到匹配返回，没有则返回null。

#### containsKey(Object key)
判断是否包含key

#### put(K key, V value) 
添加键值对

#### putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict)
