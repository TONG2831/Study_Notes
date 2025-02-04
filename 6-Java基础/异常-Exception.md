# Exception
## 1.异常类关系图
![image](http://www.plantuml.com/plantuml/png/TOynJWCn44Lxdy9Alpa1HHheefL8e9yu4wsLMHkDnmGG5TVW57IHGDncnJKG6M0jitJZ_xurynP9W2NtPDPU2EritN4ym8Qm6TwUXkFnu-LrU7wV6Hy4UGSExGhP0_c7hv9n12bP_JI-pvaZ_ynt3c_4n_3zlin1V8zrV1LSXswFsn709Wc34wkDQy-IWPRWEX-mND5cQNTBx2FVb_5AmEtRM-GBYWS5fZpUdZtEvyNjEYbe6fMemiBcTr9GQrLyoi-SDfpmbDlVJBkrxR_52Vy5XctkwHC0)

## 2.异常类说明

#### 1.Throwable
异常对象都是派生于`Throwable`。Throwable类是所有异常或错误的超类，它有两个子类：Error和Exception，分别表示错误和异常。其中异常Exception分为运行时异常(RuntimeException)和非运行时异常，也称之为不检查异常(Unchecked Exception)和检查异常(Checked Exception)。


#### 2.Error
一般是指java虚拟机相关的问题，如系统崩溃、虚拟机出错误、动态链接失败等，这种错误无法恢复或不可能捕获，将导致应用程序中断，通常应用程序无法处理这些错误，因此应用程序不应该捕获Error对象，也无须在其throws子句中声明该方法抛出任何Error或其子类。

#### 3.Exception
通常，Java的异常(包括Exception和Error)分为可查的异常（checked exceptions）和不可查的异常（unchecked exceptions）。
可查异常（编译器要求必须处置的异常）：正确的程序在运行中，很容易出现的、情理可容的异常状况。可查异常虽然是异常状况，但在一定程度上它的发生是可以预计的，而且一旦发生这种异常状况，就必须采取某种方式进行处理。
除了RuntimeException及其子类以外，其他的Exception类及其子类都属于可查异常。这种异常的特点是Java编译器会检查它，也就是说，当程序中可能出现这类异常，要么用try-catch语句捕获它，要么用throws子句声明抛出它，否则编译不会通过。
不可查异常(编译器不要求强制处置的异常):包括运行时异常（RuntimeException与其子类）和错误（Error）。

如果使用throw在方法体中抛出可查异常，则需要在方法头部声明方法可能抛出的异常类型。程序会在throw语句后立即终止，它后面的语句执行不到，然后在包含它的所有try块中（可能在上层调用函数中）从里向外寻找含有与其匹配的catch子句的try块。

#### 4.可检查异常
(1)运行时异常都是RuntimeException类及其子类异常，如NullPointerException、IndexOutOfBoundsException等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。

当出现RuntimeException的时候，我们可以不处理。当出现这样的异常时，总是由虚拟机接管。比如：我们从来没有人去处理过NullPointerException异常，它就是运行时异常，并且这种异常还是最常见的异常之一。 
出现运行时异常后，如果没有捕获处理这个异常（即没有catch），系统会把异常一直往上层抛，一直到最上层，如果是多线程就由Thread.run()抛出，如果是单线程就被main()抛出。抛出之后，如果是线程，这个线程也就退出了。如果是主程序抛出的异常，那么这整个程序也就退出了。运行时异常是Exception的子类，也有一般异常的特点，是可以被catch块处理的。只不过往往我们不对他处理罢了。也就是说，你如果不对运行时异常进行处理，那么出现运行时异常之后，要么是线程中止，要么是主程序终止。 
如果不想终止，则必须捕获所有的运行时异常，决不让这个处理线程退出。队列里面出现异常数据了，正常的处理应该是把异常数据舍弃，然后记录日志。不应该由于异常数据而影响下面对正常数据的处理。


(2)非运行时异常是RuntimeException以外的异常，类型上都属于Exception类及其子类。如IOException、SQLException等以及用户自定义的Exception异常。对于这种异常，JAVA编译器强制要求我们必需对出现的这些异常进行catch并处理，否则程序就不能编译通过。所以，面对这种异常不管我们是否愿意，只能自己去写一大堆catch块去处理可能的异常。

#### 5.常见RuntimeException异常
- ArrayStoreException                试图将错误类型的对象存储到一个对象数组时抛出的异常
- ClassCastException                试图将对象强制转换为不是实例的子类时，抛出该异常
- IllegalArgumentException         抛出的异常表明向方法传递了一个不合法或不正确的参数
- IndexOutOfBoundsException   指示某排序索引（例如对数组、字符串或向量的排序）超出范围时抛出
- NoSuchElementException       表明枚举中没有更多的元素
- NullPointerException                当应用程序试图在需要对象的地方使用 null 时，抛出该异常



