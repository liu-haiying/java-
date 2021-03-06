# Java基础
> *1. static变量与普通变量有什么区别？*
    > + 所属目标不同 static属于类的变量 普通变量属于对象
    > + 存储区域不同 静态变量存储在方法区的静态区，普通变量存储在堆中
    > + 加载时间不同静态变量随着类的加载而加载随着类的消失而消失，普通变量随着对象的加载而加载，随着对象的消失而消失
    > + 调用方式不同 静态变量通过类名或者对象（不推荐对象调用）调用 普通变量只能通过对象调用

> *2. ==与equals(很重要)*
    > - ==: 它的作用是判断两个对象的地址是不是相等。即判断两个对象是不是同一个对象（对于基本数据类型比较的是值，对于引用数据类型比较的是内存地址）
    > - equals(): 他的作用也是判断两个对象是否相等。但他一般有两种使用情况：
    >> - 情况1: 类没有覆盖equals()方法。则通过equals()比较两个对象时，等价于通过于==比较这两个对象。
    >> - 情况2：类覆盖了equals()方法。一般，我们都覆盖equals()方法来比较两个对象的内容是否相等；若他们内容相等，则返回true(即，认为这两个对象相等)

> *3. 为什么重写equals的时候要重写hashcode方法*
    > - hashCode()介绍：hashCode()的作用是获取哈希码，也称为散列码；它实际上是返回一个int整数。这个哈希码的作用是确定对象在哈希表中的索引位置。hashCode()定义在JDK的Object类中，这就意味着Java中的任何类都包含hashCode()方法。另外需要注意的是：Object的hashCode方法是本地方法，也就是用C语言或者C++实现的，该方法通常用来将对象的内存地址转换为整数之后返回。散列表存储的是键值对（key-value），它的特点是：能根据“键”快速的检索出对应的“值”。这其中就用到了散列码！（可快速找到所需要的对象）
    > - 为什么要有hashCode?\
    > 我们以“HashSet如何检查重复”为例子来说明为什么要有hashCode? 当你把对象加入HashSet时，HashSet会先计算对象的hashCode值来判断对象加入的位置，同时也会与其他已经加入的对象的hashcode值做比较，如果没有相同的hashcode，HashSet会假设对象没有重复出现。但是如果发现有相同的hashcode值的对象，这时会调用equals方法来检查hashcode相等的对象是否真的相同。如果两者相同，HashSet就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样就大大减少equals的次数，相应就大大提高了执行速度。
    > - 为什么重写equals时必须重写hashcode方法?\
    > 如果两个对象相等，则hashcode一定也是相同的。两个对象相等，对两个对象分别调用equals方法都返回true。但是，两个对象有相同的hashcode值，他们不一定是相等的。因此，equals方法被覆盖过，则hashCode方法也必须被覆盖。（hashCode()的默认行为是对堆上的对象产生独特值，如果没有重写hashCode()，则该class的两个对象无论如何都不会相等，即使这两个对象指向相同的数据）
    > 因为hashCode()所使用的杂凑算法也许刚好会让多个对象传回相同的杂凑值。越糟糕的杂凑算法越容易碰撞，但这也与数据值域分布的特性有关（所谓的碰撞也就是指的是不同的对象得到相同的hashcode），我们刚刚提到了HashSet，如果HashSet在对比的时候，同样的hashcode有多个对象，它会使用equals来判断是否真的相同。也就是说hashcode只是用来缩小查找成本的。

> *4. Java序列化中如果有些字段不想进行序列化，怎么办?*
    > - 对不想进行序列化的变量，使用transient关键字修饰。transient关键字的作用是：阻止实例中那些用此关键字修饰的变量序列化；当对象被反序列化时，被transient修饰的变量值不会被持久化和恢复。transient只能修饰变量，不能修饰类和方法。

> *5. ArrayList扩容机制*
    > - ArrayList扩容发生在add()方法调用的时候，调用ensureCapacityInternal()来扩容，通过calculateCapacity(elementData, minCapacity)获取需要扩容的长度；ensureExplicitCapacity()方法可以判断是否需要扩容，ArrayList扩容的关键方法是grow(),获取到ArrayList中的elementData数组的内存空间长度扩容至原来的1.5倍，调用Arrays.copyof将elementData数组指向新的内存空间时newCapacity的连续空间从此方法中我们可以清晰的看出其实ArrayList扩容的本质就是计算出新的扩容数组的size后实例化，并将原有数组内容复制到新数组中去。

> *6. HashMap与HashSet的区别*
    > - HashSet底层通过HashMap实现的，大部分方法都是直接调用HashMap的方法。
    > - HashSet实现Set接口，HashMap实现Map接口
    > - HashSet只存储对象，HashMap存储键值对
    > - HashSet调用add添加元素，HashMap调用put添加元素
    > - HashMap通过键值（Key）计算hashcode，HashSet通过成员对象计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法来判断对象的相等性。

> *7. HashSet如何检查重复*
    > - 当你把对象加入HashSet的时候，HashSet会先计算对象的hashcode值来判断对象的加入位置，同时也会与加入的其它对象的hashcode作比较，如果没有相同的hashcode，HashSet会假设对象没有重复出现。如果发现有相同的hashcode值的对象，这是会调用equals方法来判断这两个对象是不是真的相等，如果两者相同，HashSet就不会让其成功加入，如果不同会将其重新散列到其他位置。
    > - hashCode()与equals()的相关规定：\
        1. 如果两个对象相等，则hashcode也一定相同。\
        2. 两个对象相等，equals返回true\
        3. 两个对象有相同的hashcode值，他们不一定是相等的\
        4. 综上，equals方法被覆盖过，hashCode()方法一定被覆盖\
        5. hashCode()的默认行为是对堆上的对象产生独特值，如果没有重写hashCode()，则该class的两个对象无论如何都不会相等，即使这两个对象指向相同的数据

> *8. HashMap的底层数据结构*
    > - jdk1.8以前\
        JDK1.8之前HashMap底层是数组和链表结合在一起使用也就是链表散列。HashMap通过Key的hashcode经过扰动函数处理过后得到hash值，然后通过(n-1) & hash判断当前元素存放的位置(这里的n指的是数组的长度)，如果当前位置存在元素的话，就判断该元素与要存入的元素的hash值与key是否相同，如果相同的话，直接覆盖，如果不相同的话就通过拉链法解决冲突。\
        扰动函数指的就是HashMap的hash方法。使用hash方法也就是扰动函数是为了防止一些实现比较差的hashCode()方法 换句话说使用扰动函数过后可以减少hash碰撞。\
        拉链法：将链表和数组相结合。也就是说创建一个链表数组，数组的每一格就是一个链表。若遇到hsah冲突将冲突值加入链表中即可。
    > - jdk1.8以后\
        相比之前的版本，JDK1.8以后在解决hash冲突上有了较大的变化，当链表长度大于阈值（默认为8）（将链表转化为红黑树之前会判断，如果当前数组的长度小于64，那么会先进行数组的扩容，而不是转化为红黑树）时，将链表转化为红黑树，以减少搜索时间

> *9. HashMap的长度为什么是2的幂次方*
    > - 为了能让HashMap存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀。Hash值的范围值是-2^31到2^31，前后加起来大概40亿的映射空间，只要hash函数映射的比较松散均匀，一般应用是很难出现碰撞的。但是一个40亿长度的数组内存是放不下的。所以这个散列值是不能直接拿来用的。用之前还要先做对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。这个数组下标的计算方法是“(n-1)&hash”。(n代表数组长度)。这也就解释了为什么HashMap的长度是2的幂次方。
    > - 这个算法应该如何设计呢？
    >   - 我们⾸先可能会想到采⽤%取余的操作来实现。但是，重点来了：“取余(%)操作中如果除数是2
        的幂次则等价于与其除数减⼀的与(&)操作（也就是说 hash%length==hash&(length-1)的前提
        是 length 是2的 n 次⽅；）。” 并且 采⽤⼆进制位操作 &，相对于%能够提⾼运算效率，这就解
        释了 HashMap 的⻓度为什么是2的幂次⽅。

> *10. HashMap多线程操作导致死循环问题*
    > - 主要原因在于 并发下的Rehash 会造成元素之间会形成⼀个循环链表。不过，jdk 1.8 后解决了这个问题，但是还是不建议在多线程下使⽤ HashMap,因为多线程下使⽤ HashMap 还是会存在其他问题⽐如数据丢失。并发环境下推荐使⽤ ConcurrentHashMap 。
    > - 解决：JDK1.8使用尾插法解决了这一问题，在数组扩容的时候添加元素可能导致出现循环链

> *11. 死锁的产生与避免*
    > - 死锁：多个线程同时被阻塞，他们中的一个或全部都在等待某个资源的释放。由于线程被无限期阻塞，因此程序不能正常终止。
    > - 死锁产生的条件：
    >   - 1. 互斥条件：该资源任意一个时刻只能由一个线程占用
    >   - 2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得资源保持不放。
    >   - 3. 不剥夺条件：线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
    >   - 4. 循环等待条件：若干线程之间形成一种头尾相接的循环等待资源关系。
    > - 如何避免线程死锁？
    >   - 1. 破坏互斥条件：这个条件我们没法破坏，因为我们用锁就是为了让他们互斥。（临界资源需要互斥访问）
    >   - 2. 破坏请求与保持条件：一次性申请全部资源
    >   - 3. 破坏不剥夺条件：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放他占有的资源。
    >   - 4. 破坏循环等待条件：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

> *12. sleep()与wait()的区别与共同点*
    > - 两者最主要的区别是：sleep()方法没有释放锁，而wait()方法释放了锁。
    > - 两者都可以暂停线程的执行。
    > - wait()通常用作线程间的交互/通信，sleep()通常用作暂停执行。
    > - wait()方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的notify()或者notifyAll()方法。sleep()方法执行完成后，线程会自动苏醒。或者可以使用wait(long time)超时后线程会自动苏醒。

> *13. 为什么我们调用start()方法时会执行run()方法，为什么我们不能直接调用run()方法*
    > - new一个Thread，线程进入了新建状态。调用start()方法，会启动一个线程并使线程进入就绪状态，当分配到时间片后就可以开始运行了。start()方法会执行线程相应的准备工作，然后自动执行run()方法的内容，这是真正的多线程工作。但是，直接执行run()方法，会把run()方法当作一个main线程下的普通方法去执行，并不会在某个线程中去执行他，所以这并不是多线程工作。
    > - 总结：调用start()方法方可启动线程进入就绪状态，直接执行run()方法的话不会以多线程的方式执行。

> *14. synchronized关键字*
    > - synchronized关键字解决的是多个线程访问资源的同步性，synchronized关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。
    > - synchronized使用的三种方式：
    >   - 修饰实例方法：作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁。
    >   - 修饰静态方法：也就是给当前类加锁，会作用于类的所有实例对象，进入同步代码前要获得当前class的锁，因为静态成员不属于任何对象实例，是类成员（static表明这是该类的一个静态资源，不管new了多少个对象，该资源只有一份）。所以，如果一个线程A调用一个实例对象的非静态synchronized方法，而线程B需要调用这个实例对象所属类的静态synchronized方法，是允许的，不会发生互斥现象，因为访问静态synchronized方法占用的锁是当前类的锁，而访问非静态的synchronized方法占用的锁是当前实例对象的锁。
    >   - 修饰代码块：指定加锁对象，对给定对象/类加锁。synchronized(this|object)表示进入同步代码块前要获得给定对象的锁。synchronized(类名.class)表示进入同步代码前要获得当前class的锁。
    > - 构造方法不能使用synchronized关键字修饰，构造方法本身就是线程安全的，不存在同步的构造方法一说。
    > - synchronized关键字的底层原理
    >   - synchronized同步语句块的实现使用的是monitorenter和monitorexit指令，其中monitorenter指令指向同步代码块的开始位置，monitorexit指令则指明同步代码块的结束位置。synchronized修饰的方法并没有monitorenter和monitorexit指令，取而代之的是ACC_SYNCHRONIZED标识，该标识指定了一个方法为一个同步方法，不过两者的本质都是对对象监视器monitor的获取。

> *15. volatile关键字的作用*
    > - 防止JVM的指令重排序
    > - 保证变量的可见性（多线程直接操作主内存的volatile字段）

> *16. synchronized关键字与volatile关键字的区别*
    > - synchronized关键字与volatile关键字是互补的存在，不是对立的存在！
    >   - volatile关键字是线程同步的轻量级实现，所以volatile性能比synchronized关键字要好。但是volatile关键字只能修饰变量，但是synchronized关键字可以修饰方法以及代码块。
    >   - volatile关键字能保证数据的可见性，但不能保证数据的原子性。synchronized关键字两者都能保证。
    >   - volatile关键字主要用于解决变量在多个线程之间的可见性，而synchronized关键字解决的是多个线程之间访问资源的同步性。

> *17. 为什么要使用线程池*
    > - 线程池提供了一种限制和管理资源（包括执行一个任务）。每个线程池还维护一些基本统计信息，例如已完成任务数量。
    > - 使用线程池的好处：
    >   - 1. 降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
    >   - 2. 提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
    >   - 3. 提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

> *18. ThreadLocal变量*
    > - 通常我们的变量是可以被任意一个线程访问并修改的，如果想实现一个没有个线程都有自己的专属变量应该如何解决？JDK中提供的ThreadLocal类就解决了这一个问题。ThreadLocal类主要就是为了让每个线程绑定自己的值，可以将ThreadLocal比作存放数据的盒子，而盒子中存放的就是每个线程私有的数据。
    > - 如果你创建了一个ThreadLocal变量，那么访问这个变量的每个线程都会有这个变量的本地副本，这也就是ThreadLocal的名字的由来，他们可以使用get()和set()方法来获取默认值或将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。

> *19. 实现Runnable接口和Callable接口的区别*
    > - Runnable接口不会返回结果或抛出检查异常，但是Callable接口可以。所以，如果任务不需要返回返回结果或抛出异常推荐使用Runnable接口，这样代码看起来会更简洁。

> *20. 执行execute()方法和submit()方法的区别是什么？*
    > - execute()方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功。（实现Runnable）
    > - submit()方法用于提交需要返回值的任务。线程池会返回一个Future类型的对象，通过这个Future对象可以判断任务是否执行成功，并且可以通过Future的get()方法来获取返回值，get()方法会阻塞当前线程直到任务完成，而使用get(long timeout, TimeUnit unit)方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。(实现Callable)
# 数据结构
> *1. 二叉查找树（BST）（也叫二叉排序树）*
    > - 二叉查找树的特性：
    >   - 1. 左子树上所有节点的值均小于或等于他的根节点的值。
    >   - 2. 右子树上所有节点的值均大于或等于他的根节点的值。
    >   - 3. 左右子树分别为二叉排序树
    > - 二叉查找树的查询最大次数即为树的高度
    > - 缺点：
    >   - 在某些情况下二叉查找树会变成接近线性的结构查找性能大打折扣

> *2. 红黑树（Red Black Tree）*
    > - 红黑树是一种自平衡的二叉查找树（为了解决二叉查找树的缺点），除了符合二叉查找树的基本特性以外，他还有下列附加特性：
    >   - 节点是红色或者黑色。
    >   - 根节点是黑色。
    >   - 每个叶子节点都是黑色的空节点（NIL节点）。
    >   - 每个红色节点的两个子节点都是黑色。（从每个叶子到根的所有路径上不能有两个连续的红色节点）
    >   - 从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。
    > - 红黑树从根到叶子的最长路径不会超过最短路径的2倍
    > - 插入或者删除节点的时候红黑树的规则可能被打破，这样就需要一些调整来继续维持规则
    >   - 变色：为了重新符合红黑树的规则，尝试把红色节点变为黑色，或者把黑色节点变为红色。
    >   - 旋转（左旋转/右旋转）：逆时针旋转红黑树的两个节点，使得父节点被自己的右孩子取代，而自己成为自己的左孩子。（右旋转反之）
# Redis
> 在生产环境中，会因为很多的原因造成访问请求绕过了缓存，都需要访问数据库持久层，虽然对Redsi缓存服务器不会造成影响，但是数据库的负载就会增大，使缓存的作用降低\
>*1. 缓存穿透*
    > - 缓存穿透指的是查询一个根本不存在的数据,缓存层和持久层都不会命中.在日常工作中出于容错的考虑,如果从持久层查不到数据则不写入缓存层,缓存穿透将导致不存在的数据每次请求都会去持久层查询,失去了缓存保护后端持久的意义.
    ![avatar](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9xcWFkYXB0LnFwaWMuY24vdHhkb2NwaWMvMC81ZWQzMTk3MDYzMjVjOTgyN2M0NDU5MDZmOWQ4YzIyOS8w?x-oss-process=image/format,png)
    > - 缓存穿透问题可能会使后端存储负载加大,由于很多后端持久层并不具备高并发性,甚至可能造成后端存储宕机.通常可以在程序中统计总调用数,缓存层命中数,如果同一个key命中数很低,就有可能发生了缓存穿透
    > - 造成缓存穿透有两个基本原因.第一就是自身的业务代码或者数据出现了问题(比如 set和get的key不一致),第二就是一些恶意的攻击,爬虫等造成大量空命中(爬取线上商城商品数据,超大循环递增商品的ID)
    > - 解决办法
    >> - 1.缓存空对象\
    缓存空对象:在持久层没有命中的情况下对key进行set(key, null)\
    缓存空对象会有两个问题: 第一就是value为null不代表不占用内存空间,空值做了缓存,意味着缓存层中存了更多的键,需要更多的内存空间,比较有效的方法就是针对这类数据设置一个较短的过期时间,让其自动剔除.第二, 缓存层和持久层的数据会有一段时间窗口的不一致,可能会对业务有一定影响.例如过期时间设置了5分钟,如果此时持久层添加了这条数据,那此段时间就会出现缓存层和持久层的数据不一致,此时可以利用消息系统或者其他方式清除掉缓存层中的空对象
    >> - 2.布隆过滤器拦截\
    在访问缓存层和存储层之前，将存在的key用布隆过滤器提前保存起来，做第一层拦截，当收到一个对key请求时先用布隆过滤器验证是key否存在，如果存在在进入缓存层、存储层。可以使用bitmap做布隆过滤器。这种方法适用于数据命中不高、数据相对固定、实时性低的应用场景，代码维护较为复杂，但是缓存空间占用少。\
    布隆过滤器实际上是一个很长的二进制向量和一系列随机映射函数。布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。
    >> - 3.使用互斥锁排队\
    也是业界比价普遍的一种做法，即根据key获取value值为空时，锁上，从数据库中load数据后再释放锁。若其它线程获取锁失败，则等待一段时间后重试。这里要注意，分布式环境中要使用分布式锁，单机的话用普通的锁（synchronized、Lock）就够了。
    > ![avatar](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9xcWFkYXB0LnFwaWMuY24vdHhkb2NwaWMvMC8wMzU5MzNlZGFhZTA3NTNhZjE2YWI3NTY4YWQ3NDJiNS8w?x-oss-process=image/format,png)

>*2. 缓存击穿*
    > - 缓存击穿是指缓存中没有但数据库中有数据(一般情况为缓存时间到期), 这时由于并发的用户特别多,同时读取缓存没有读取到数据,又同时去数据库中去取数据,引起数据库压力瞬间增大,造成过大压力,甚至有可能造成应用崩溃.
    > - 解决办法
    >> - 1. 使用互斥锁(mutex key)\
    业界比较常用的办法就是使用mutex.简单的说就是在缓存失效的时候(判断拿出来的值为空), 不是去立即load db, 而是先使用缓存工具的某些带成功操作返回值的的操作(比如redis的SETNX或者Memcache的ADD)去set一个mutex key,当操作返回成功的时候再去load db的操作并回设缓存;否则,就重试整个get缓存的方法.
    >> - 2. 设置热点数据永不过期


>*3. 缓存雪崩*
    > - 缓存雪崩是指缓存中的数据大批量过期,而查询数据量巨大,引起数据库压力过大甚至宕机.和缓存击穿不同的是,缓存击穿指并发查同一条数据,缓存雪崩是不同数据都过期了,很多数据都查不到从而去查数据库.\
    > - 解决方案
    >> - 1. 缓存数据的过期时间设置随机,防止同一时间大量数据过期现象发生.
    >> - 2. 如果缓存数据库是分布式部署,将热点数据均匀分布在不同的缓存数据库中.
    >> - 3. 设置热点数据永不过期

# MySQL优化
> 在MySQL中可以使用show status语句查询MySQL数据库的一些性能参数。其语法如下：
Show status like ‘value’;
其中value是要查询的参数值，常用的一些性能参数如下：\
Connections ：连接MySQL服务器的时间\
Uptime ： MySQL服务器的上线时间\
Slow_ queries ：慢查询的次数\
Com_select ： 查询操作的次数\
Com_insert ：插入操作的次数\
Com_update：更新操作的次数\
Com_delete ：删除操作的次数\

> MySQL性能优化主要分为两个方面优化查询和优化表结构\
> *1.优化查询*
>> - 使用索引,使用连接查询代替子查询,可以使用MySQL自带的关键字explain或describe（desc）可以对查询语句进行分析.
>> - 使用索引 
>>> - 1. 如果在成绩表中给score字段设置索引，但查询时where条件使用的是一个范围而不是一个确切的值时，索引无效
>>> - 2. 在子查询中，主句连接的索引无效，子句连接的索引有效
>>> - 3. 使用like关键字进行查询时，如果匹配字符串的第一个字符为“%”，索引不会起作用，“%”不在第一个位置索引才会起作用 
>>> - 4. 使用多列索引查询语句,对于多列索引，只有在查询条件中使用了这些字段中的第一个字段时，索引才会被使用。
>>> - 5. 使用or关键字的查询语句, 查询语句的条件中只有OR时，并且OR前后的俩个条件中的列都是索引时，查询中才使用索引，否则不使用索引。
>> - 优化子查询,使用连接查询（join）代替子查询可以提高查询效率。
> *2.优化数据库表结构*
>>> - 1. 将字段较多的表分解成多个表, 对于字段较多的表，如果有些字段的使用频率很低，可以将这些字段分离出来形成新表。因为当一个表的数据量很大时，会由于存在使用频率低的字段而使查询速度变慢。
>>> - 2. 增加中间表, 对于经常需要联合查询的表，可以建立中间表以提高查询效率。通过建立中间表，把需要经常联合查询的数据插入中间表，然后将原来的联合查询改为对中间表的查询，以此来提高查询效率。
>>> - 3. 增加冗余字段, 设计数据库表时应尽量遵循范式理论的规约，尽可能减少冗余字段，让数据库表的设计看起来精致、优雅。但是，合理地加入冗余字段可以提高查询速度。如：员工信息存储在staff表中，部门信息存储在department表中。通过staff表中的department_id字段与department表建立关系，如果要查询一个员工所在的部门的名称，必须从staff表中找到员工所在部门的编号（department_id），然后根据这个编号在department表中查找部门名称，如果经常需要这个操作，连接查询便会浪费很多时间。可以在staff表中增加一个冗余字段department_name，该字段用于存储员工所在部门的名称，这样就不用每次都进行连接查询操作了。
>>> - 4. 优化插入记录的速度 innoDB引擎的表常见的优化方法
>>>> + 1. 禁用外键检查,插入数据之前禁止对外键的检查，数据插入完成之后再恢复对外键的检查。Set foreign_key_checks=0; 恢复外键检查:set foreign_key_checks=1;
>>>> + 2. 禁用唯一性检查,插入数据时，MySQL会对插入的记录进行唯一性校验。这种唯一性校验会降低插入记录的速度。为了降低这种情况对查询速度的影响可以在插入记录之前禁用唯一性检查，等到记录插入完毕后再开启。Set  unique_check=0; 开启唯一性检查的语句如下：set  unique_checks=1;
>>>> + 3. 禁止自动提交, 插入数据之前禁止事务的自动提交，数据导入之后，执行恢复自动提交操作。禁止自动提交的语句 set autocommit=0;恢复自动提交:set autocommit=1;
>>> 5. 分析、检查和优化表


# Java集合框架
集合的特点：
- 集合是用于存储对象的容器，对象是用来封装数据的，但对象多了也需要统一管理。
- 集合长度可变，数组长度需事先定义。

集合与数组的区别：
- 数组长度固定；集合长度可变。
- 数组可以存储基本数据类型，也可以存储引用数据类型。集合只能存储引用数据类型。
- 数组存储元素必须是同一个类型的数据，集合可以存储不同类型。

集合框架全继承图\
Collection
![avatar](https://img2018.cnblogs.com/blog/1362965/201901/1362965-20190118094735724-2129767713.png)

Map
![avatar](https://img2018.cnblogs.com/blog/1362965/201901/1362965-20190118095106326-273814633.png)


## List、Set、Map三者的区别？
- Java容器分为Collection、Map两大类，Collection集合的子接口有Set、List、Queue三种子接口。我们比较常用的是Set、List, Map不是collection的子接口而是一个独立的接口。
- Collection集合主要有List和Set两大接口
    - List: 一个有序（元素存入集合的顺序和取出的顺序一致）容器，元素可以重复，可以插入多个null元素，元素都有索引，常用的实现类有ArrayList、LinkedList和Vector。
    - Set: 一个无序（元素存入集合的顺序与取出顺序不一致）容器，不可以存储重复元素，只允许一个为null的元素，必须保证元素唯一性。Set接口常用实现类HashSet、LinkedHashSet以及TreeSet.
    - Map: 是一个键值对集合，存储键、值和之间的映射。Key无序，唯一；value不要求有序，允许重复。Map没有继承于Collection接口，从Map集合中检索元素时只要给出键对象，就会返回对应的值对象。
