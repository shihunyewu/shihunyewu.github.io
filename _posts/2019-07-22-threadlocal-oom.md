thread local 为何会发生内存泄漏？

threadlocalMap 中的 key 是 weak reference。

当系统回收了 key 之后，原本的 key 就变成了 null，但是 value 值无法通过 null key 访问到（因为用户只会通过 object key 来访问 value，但是原来的 object key 变成 null 了）。

因为 key 为 null，value 对象无法 gc root，value 对象就无法销毁，只要线程存在，这个对象就一直存在，因为 threadLocal 有强引用这个 value。

ThreadLocal内存泄漏的根源是：由于ThreadLocalMap的生命周期跟Thread一样长，如果没有手动删除对应key的value就会导致内存泄漏，而不是因为弱引用。

#### 参考
1. [ThreadLocal](https://www.jianshu.com/p/a1cd61fa22da)