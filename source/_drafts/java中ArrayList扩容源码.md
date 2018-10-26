---
title: java中ArrayList扩容源码
date: 2018-09-20 22:21:59
categories:
    - java
tags:
    - 源码
---
## ArrayList最大长度是多大
有个问题很好奇，ArrayList最大长度是多大，看了下源码记录下。
ArrayList在添加元素时，先是确保有容量添加元素，如果没有容量会先进行扩容。如果是空数组会初始化10个长度。数组的最大长度是Integer.MAX_VALUE

1. 确保有容量源码
```
private void ensureCapacityInternal(int minCapacity) {
        //如果是空数组会初始化10个长度
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
}
```


2. 扩容源码
```
    /**
     * The maximum size of array to allocate.
     * Some VMs reserve some header words in an array.
     * Attempts to allocate larger arrays may result in
     * OutOfMemoryError: Requested array size exceeds VM limit
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
    
    /**
     * Increases the capacity to ensure that it can hold at least the
     * number of elements specified by the minimum capacity argument.
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

## 为什么MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8
有些虚拟机存储header words，避免一些机器内存溢出，减少出错几率，所以少分配。
最大还是能支持到Integer.MAX_VALUE