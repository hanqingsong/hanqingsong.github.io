---
title: java从字节码看finally执行过程
date: 2018-09-18 18:38:10
categories:
  - java
tags:
  - java
---
## 问题
以下代码执行结果？如果把注释代码打开执行结果？如果有疑惑就往下看。
```
public class TestFinally {
    public int foo() {
        int x;
        try {
            x = 2;
            // x = x/0;
            return x;
        } catch (Exception e) {
            x = 3;
            return x;
        } finally {
            x = 4;
        }
    }

    public static void main(String[] args) {
        int foo = new TestFinally().foo();
        System.out.println(foo);
    }
}
```
答案：输出 2，打开注释输出3。

## finally 语句块执行过程
Java 虚拟机会把 finally 语句块作为 subroutine 直接插入到 try 语句块或者 catch 语句块的控制转移语句之前。但是，还有另外一个不可忽视的因素，那就是在执行 subroutine（也就是 finally 语句块）之前，try 或者 catch 语句块会**保留其返回值到本地变量表**（Local Variable Table）中。待 subroutine 执行完毕之后，再恢复保留的返回值到操作数栈中，然后通过 return 或者 throw 语句将其返回给该方法的调用者（invoker）。只适用于 return 和 throw 语句，不适用于 break 和 continue 语句，因为它们根本就没有返回值。
具体过程需要看Java编译的字节码。

## 查看字节码命令
使用javap命令查看字节码
```
 ✗ javap -help
用法: javap <options> <classes>
其中, 可能的选项包括:
  -help  --help  -?        输出此用法消息
  -version                 版本信息
  -v  -verbose             输出附加信息
  -l                       输出行号和本地变量表
  -public                  仅显示公共类和成员
  -protected               显示受保护的/公共类和成员
  -package                 显示程序包/受保护的/公共类
                           和成员 (默认)
  -p  -private             显示所有类和成员
  -c                       对代码进行反汇编
  -s                       输出内部类型签名
  -sysinfo                 显示正在处理的类的
                           系统信息 (路径, 大小, 日期, MD5 散列)
  -constants               显示最终常量
  -classpath <path>        指定查找用户类文件的位置
  -cp <path>               指定查找用户类文件的位置
  -bootclasspath <path>    覆盖引导类文件的位置

 ✗ javap -verbose -p TestFinally
```

## 生成字节码
```
javac TestFinally.java
javap -verbose -p TestFinally #可以不带.class
```
```
Classfile /Users/hanqingsong/work/eclipse_workspace/testproject/src/main/java/javap/TestFinally.class
  Last modified 2018-9-18; size 655 bytes
  MD5 checksum 2861ffb88c526670a82caa7af5ca36a3
  Compiled from "TestFinally.java"
public class javap.TestFinally
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #8.#22         // java/lang/Object."<init>":()V
   #2 = Class              #23            // java/lang/Exception
   #3 = Class              #24            // javap/TestFinally
   #4 = Methodref          #3.#22         // javap/TestFinally."<init>":()V
   #5 = Methodref          #3.#25         // javap/TestFinally.foo:()I
   #6 = Fieldref           #26.#27        // java/lang/System.out:Ljava/io/PrintStream;
   #7 = Methodref          #28.#29        // java/io/PrintStream.println:(I)V
   #8 = Class              #30            // java/lang/Object
   #9 = Utf8               <init>
  #10 = Utf8               ()V
  #11 = Utf8               Code
  #12 = Utf8               LineNumberTable
  #13 = Utf8               foo
  #14 = Utf8               ()I
  #15 = Utf8               StackMapTable
  #16 = Class              #23            // java/lang/Exception
  #17 = Class              #31            // java/lang/Throwable
  #18 = Utf8               main
  #19 = Utf8               ([Ljava/lang/String;)V
  #20 = Utf8               SourceFile
  #21 = Utf8               TestFinally.java
  #22 = NameAndType        #9:#10         // "<init>":()V
  #23 = Utf8               java/lang/Exception
  #24 = Utf8               javap/TestFinally
  #25 = NameAndType        #13:#14        // foo:()I
  #26 = Class              #32            // java/lang/System
  #27 = NameAndType        #33:#34        // out:Ljava/io/PrintStream;
  #28 = Class              #35            // java/io/PrintStream
  #29 = NameAndType        #36:#37        // println:(I)V
  #30 = Utf8               java/lang/Object
  #31 = Utf8               java/lang/Throwable
  #32 = Utf8               java/lang/System
  #33 = Utf8               out
  #34 = Utf8               Ljava/io/PrintStream;
  #35 = Utf8               java/io/PrintStream
  #36 = Utf8               println
  #37 = Utf8               (I)V
{
  public javap.TestFinally();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0  // 将第一个引用类型本地变量推送至栈顶
         1: invokespecial                   // Method java/lang/Object."<init>":()V 调用超类构造方法
         4: return
      LineNumberTable:
        line 3: 0

  public int foo();
    descriptor: ()I
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=5, args_size=1
         0: iconst_2 // 将int类型 2 推送至栈顶
         1: istore_1 // 将栈顶int型数值存入第二个本地变量
         2: iload_1   // 将第二个int型本地变量推送至栈顶
         3: istore_2 // 将栈顶int型数值存入第三个本地变量（这里！！！）
         4: iconst_4 // 将int类型4推送至栈顶
         5: istore_1 // 将栈顶int类型数值存入第二个本地变量
         6: iload_2 // 将第三个int型本地变量推送至栈顶
         7: ireturn // 从当前方法返回int

          8: astore_2 // 将栈顶引用型数值存入第三个本地变量
         9: iconst_3 // 将int型3推送至栈顶
        10: istore_1 // 将栈顶int型数值存入第二个本地变量
        11: iload_1 // 将第二个int型本地变量推送至栈顶
        12: istore_3 // 将栈顶int类型数值存入第四个本地变量（这里！！！）
        13: iconst_4 // 将int类型4推送至栈顶
        14: istore_1 // 将栈顶int型数值存入第二个本地变量
        15: iload_3 // 将第四个本地变量推送至栈顶
        16: ireturn // 从当前方法返回int
        
        17: astore        4 // 将栈顶引用型数值存入第五个本地变量
        19: iconst_4        // 将int型4推送至栈顶
        20: istore_1        // 将栈顶int型数值存入第二个本地变量
        21: aload         4 // 将第五个应用类型本地变量推送至栈顶
        23: athrow          // 将栈顶异常抛出
      Exception table:
         from    to  target type
             0     4     8   Class java/lang/Exception
             0     4    17   any
             8    13    17   any
            17    19    17   any
      LineNumberTable:
        line 7: 0
        line 8: 2
        line 13: 4
        line 8: 6
        line 9: 8
        line 10: 9
        line 11: 11
        line 13: 13
        line 11: 15
        line 13: 17
      StackMapTable: number_of_entries = 2
        frame_type = 72 /* same_locals_1_stack_item */
          stack = [ class java/lang/Exception ]
        frame_type = 72 /* same_locals_1_stack_item */
          stack = [ class java/lang/Throwable ]

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=2, args_size=1
         0: new           #3                  // class javap/TestFinally
         3: dup
         4: invokespecial #4                  // Method "<init>":()V
         7: invokevirtual #5                  // Method foo:()I
        10: istore_1
        11: getstatic     #6                  // Field java/lang/System.out:Ljava/io/PrintStream;
        14: iload_1
        15: invokevirtual #7                  // Method java/io/PrintStream.println:(I)V
        18: return
      LineNumberTable:
        line 18: 0
        line 19: 11
        line 20: 18
}
SourceFile: "TestFinally.java"
```

关于虚拟机字节码指令表，可以在《深入理解Java虚拟机 JVM高级特性与最佳实践-附录B》中获取
## 参考资料
https://juejin.im/post/5aca2c366fb9a028c97a5609
http://www.54tianzhisheng.cn/2017/06/13/%E6%B7%B1%E5%BA%A6%E6%8E%A2%E7%A9%B6Java%20%E4%B8%AD%20finally%20%E8%AF%AD%E5%8F%A5%E5%9D%97/