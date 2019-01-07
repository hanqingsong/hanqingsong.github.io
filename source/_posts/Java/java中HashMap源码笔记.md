---
title: java中HashMap源码笔记
categories:
  - java
tags:
  - java
date: 2019-01-01 09:33:24
---
## HashMap概述
HashMap基于哈希表的 Map 接口的实现。并允许使用 null 值和 null 键。（除了不同步和允许使用 null 之外，HashMap 类与 Hashtable 大致相同。）
HashMap的数据结构使用数组和链表来实现对数据的存储，jdk1.8之后链表节点达到8之后转为红黑树。默认初始化容量为16（DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16），扩容因子0.75（DEFAULT_LOAD_FACTOR = 0.75f），树化链表值是8（TREEIFY_THRESHOLD = 8）。
HashMap为线程不安全，多线程操作会导致其死循环。


## 数组+链表数据结构
HashMap的底层主要是基于数组和链表来实现的，它之所以有相当快的查询速度主要是因为它是通过计算散列码来决定存储的位置。如果存储的对象对多了，就有可能不同的对象所算出来的hash值是相同的，这就出现了所谓的hash冲突。解决hash冲突的方法有很多，HashMap底层是通过链表来解决hash冲突的。
数组的特点是：寻址容易，插入和删除困难。
链表的特点是：寻址困难，插入和删除容易。

![](http://ww1.sinaimg.cn/large/99c92116ly1fyxs8t9ao1j20f10co75l.jpg)

## put插入方法
put数据过程
1. key计算hash值
2. 找位置，根据hash值取模找对应的数组中位置，（取模操作(n - 1) & hash，n为数组的长度，相当于hash%n，位运算效率更高 ）
3. 放入数据，如果为空放入数组，如果有hash碰撞，放入链表中，如果链表长度超过8，转化为红黑树
4. 判断大小是否需要扩容

先把key做下hash值计算：hash(key)
```
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
```

根据hash值判断放入数组链表相应位置中，
```
/**
 * Implements Map.put and related methods
 *
 * @param hash hash for key
 * @param key the key
 * @param value the value to put
 * @param onlyIfAbsent if true, don't change existing value
 * @param evict if false, the table is in creation mode.
 * @return previous value, or null if none
 */
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        // tab 数组，p指向节点， n数组长度，i取模索引位置
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        // 数组没有创建初始化数组
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        // 找放入的位置
        // 1. 如果数组hash对应索引元素为空，创建数据赋值
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            // 2. 如果和第一个节点hash值相同，key值相同，已存在
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            // 3. 如果是红黑树节点，放入树节点
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
            // 4. hash冲突，放到链表最后一个节点，或者转换为树 
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            // 如果已经存在，则替换为新value
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        // 判断大小是否超过阀值
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

## get查询方法
get数据过程
1. key计算hash值
2. 找位置，如果第一key值相同，直接返回，不相同遍历链表中数据

```
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```

```
/**
 * Implements Map.get and related methods
 *
 * @param hash hash for key
 * @param key the key
 * @return the node, or null if none
 */
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    // (n - 1) & hash 就是取模定位数组的索引
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            // 数组中找到，返回
            return first;
        // 找链表中数据
        if ((e = first.next) != null) {
            if (first instanceof TreeNode)
            // 递归树查找
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
            // 遍历链表
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}

```
## 数组长度为什么总是设定为 2 的 N 次方？
1. 取模快。
其实就是上面为什么快的原因：位与取模比 % 取模要快的多。
2. 分散平均，减少碰撞。 
这个是主要原因。
如果二进制某位包含 0，则此位置上的数据不同对应的 hash 却是相同，碰撞发生，而 (2^x - 1) 的二进制是 0111111…，分散非常平均，碰撞也是最少的。


## 解决hash冲突的办法
1. 开放定址法（线性探测再散列，二次探测再散列，伪随机探测再散列）
2. 再哈希法
3. 链地址法
4. 建立一个公共溢出区

hashmap的解决办法就是采用的链地址法

## 参考资料
https://blog.csdn.net/fighterandknight/article/details/61624150
http://jayfeng.com/2016/12/28/%E7%90%86%E8%A7%A3HashMap/