# Java四中引用类型

参考 https://www.jianshu.com/p/ca6cbc246d20

> 在学习ThreadLocal时,发现ThreadLocal是弱引用.处于对弱引用的好奇,所以进一步了解有四中:`强引用`,`软引用`,`弱引用`,`虚引用`

### 为什么需要不同的引用类型?

> 从Java1.2开始，JVM开发团队发现，单一的强引用类型，无法很好的管理对象在JVM里面的生命周期，垃圾回收策略过于简单，无法适用绝大多数场景。为了更好的管理对象的内存，更好的进行垃圾回收，JVM团队扩展了引用类型，从最早的强引用类型增加到**强、软、弱、虚**四个引用类型。 

 ### 引用类图

![images\引用.jpg](images\引用.jpg)

StrongRerence为JVM内部实现。其他三类引用类型全部继承自Reference父类。

###  **一.强引用（StrongReference）**

 最常用到的引用类型，StrongRerence这个类并不存在，而是在JVM底层实现。默认的对象都是强引用类型，继承自Rerence、SoftReference、WeakReference、PhantomReference的引用类型非强引用。

最简单的强引用示例:

```
String str = "Misout的博客";
```

强引用类型，如果JVM垃圾回收器GC Roots可达性分析结果为可达，表示引用类型仍然被引用着，这类对象始终不会被垃圾回收器回收，即使JVM发生OOM也不会回收。而如果GC Roots的可达性分析结果为不可达，那么在GC时会被回收。

 ### **二.软引用（SoftReference）**

软引用是一种比强引用生命周期稍弱的一种引用类型。在JVM内存充足的情况下，软引用并不会被垃圾回收器回收，只有在JVM内存不足的情况下，才会被垃圾回收器回收。所以软引用的这种特性，一般用来实现一些内存敏感的缓存，只要内存空间足够，对象就会保持不被回收掉，比如网页缓存、图片缓存等。

**软引用使用示例**

```
SoftReference<String> softReference = new SoftReference<String>(new String("Misout的博客"));
System.out.println(softReference.get());
```

### 三. **弱引用（WeakReference）**

弱引用是一种比软引用生命周期更短的引用。他的生命周期很短，不论当前内存是否充足，都只能存活到下一次垃圾收集之前。
 **来让我们看一个示例**

```
WeakReference<String> weakReference = new WeakReference<String>(new String("Misout的博客"));
System.gc();
if(weakReference.get() == null) {
    System.out.println("weakReference已经被GC回收");
}
```

输出结果：

```
weakReference已经被GC回收
```

从结果中能看出其存活的周期。

### **四.虚引用（PhantomReference）**

虚引用与前面的几种都不一样，这种引用类型不会影响对象的生命周期，所持有的引用就跟没持有一样，随时都能被GC回收。需要注意的是，在使用虚引用时，必须和引用队列关联使用。在对象的垃圾回收过程中，如果GC发现一个对象还存在虚引用，则会把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象内存被回收之前采取必要的行动防止被回收。虚引用主要用来跟踪对象被垃圾回收器回收的活动。
 **示例**

```
PhantomReference<String> phantomReference = new PhantomReference<String>(new String("Misout的博客"), new ReferenceQueue<String>());
System.out.println(phantomReference.get());
```

运行后，发现结果总是null，引用跟没有持有差不多

 **总结**

| 类型   | 回收时间                                | 使用场景                                                     |
| ------ | --------------------------------------- | ------------------------------------------------------------ |
| 强引用 | 一直存活，除非GC Roots不可达            | 所有程序的场景，基本对象，自定义对象等。                     |
| 软引用 | 内存不足时会被回收                      | 一般用在对内存非常敏感的资源上，用作缓存的场景比较多，例如：网页缓存、图片缓存 |
| 弱引用 | 只能存活到下一次GC前                    | 生命周期很短的对象，例如ThreadLocal中的Key。                 |
| 虚引用 | 随时会被回收， 创建了可能很快就会被回收 | 业界暂无使用场景， 可能被JVM团队内部用来跟踪JVM的垃圾回收活动 |

以上这些引用可能对大部分人来说都不会用到，但不管怎样，必须要了解以上四种引用类型的概念和区别，以及生存周期。

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 