---
title: java中对象克隆
categories:
  - java
tags:
  - java
date: 2018-06-13 07:39:47
---


## 浅克隆
在Java语言中，所有的Java类都继承自java.lang.Object。事实上，Object类提供一个clone()方法，可以将一个Java对象复制一份。因此在Java中可以直接使用Object提供的clone()方法来实现对象的克隆。需要注意的是能够实现克隆的类必须实现接口Cloneable。
```
Object object = null;
try {
　　　object = super.clone();
} catch (CloneNotSupportedException exception) {
　　　System.err.println("Not support cloneable");
}
return (Prototype )object;
```

## 深克隆
在Java语言中，如果需要实现深克隆，可以通过序列化(Serialization)等方式来实现。序列化就是将对象写到流的过程，写到流中的对象是原有对象的一个拷贝，而原对象仍然存在于内存中。通过序列化实现的拷贝不仅可以复制对象本身，而且可以复制其引用的成员对象，因此通过序列化将对象写到一个流中，再从流里将其读出来，可以实现深克隆。需要注意的是能够实现序列化的对象其类必须实现Serializable接口，否则无法实现序列化操作。
```
//将对象写入流中
ByteArrayOutputStream bao=new  ByteArrayOutputStream();
ObjectOutputStream oos=new  ObjectOutputStream(bao);
oos.writeObject(this);

//将对象从流中取出
ByteArrayInputStream bis=new  ByteArrayInputStream(bao.toByteArray());
ObjectInputStream ois=new  ObjectInputStream(bis);
return  (WeeklyLog)ois.readObject();

```

## 资料
原型模式 https://blog.csdn.net/lovelion/article/details/7424623