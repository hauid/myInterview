

### StringBuilder 和 StringBuffer 

二者之间的最大不同在于 **StringBuilder 的方法不是线程安全**的（没有 `synchronized` 修饰，多个线程可以同时调用它的方法，可能导致数据不一致（非线程安全）, 不能同步访问）,StringBuffer 是线程安全的，它的所有公开方法都是同步的，**StringBuilder 没有对方法加锁Synchronized**,,这里的同步和异步是什么意思,同步和异步操作中的"同步"与"异步"和这里的是一样吗

**在编程中，“同步”和“异步”这两个术语的具体含义会根据上下文有所不同。以下是详细的解释：**

---

#### **1. `StringBuffer` 和 `StringBuilder` 中的“同步”**

- **同步（Synchronized）**
  这里的“同步”是 **线程同步（Thread Synchronization）** 的概念，目的是解决多线程环境下的资源竞争问题。  
  - `StringBuffer` 的同步：它的所有公开方法都标记了 synchronized` 关键字，表示同一时间只允许一个线程访问这些方法。  
  - `StringBuilder` 的非同步：没有 `synchronized` 修饰，多个线程可以同时调用它的方法，可能导致数据不一致（非线程安全）。

---

#### **2. 一般操作中的“同步”和“异步”**(代码同步与异步)

这里的“同步”和“异步”通常描述的是 **代码执行的模式**，与线程安全无关：

- 同步操作（Synchronous）  
  代码按顺序执行，每一步操作必须等待前一步完成后才能继续。例如：  

  ```java
  // 同步读取文件：代码会阻塞，直到文件读取完成
  String content = readFileSync("file.txt");
  System.out.println(content);
  ```

  在等待期间，**当前线程会被阻塞（Blocked）**，无法执行其他任务。

- 异步操作（Asynchronous）  
  代码发起一个操作后，不需要等待其完成，可以继续执行后续任务。操作完成后，通过回调（Callback）或事件通知（Event）处理结果。例如：  

  ```java
  // 异步读取文件：立即返回，读取完成后回调
  readFileAsync("file.txt", (content) -> {
      System.out.println(content);
  });
  System.out.println("继续执行其他任务...");
  ```

  异步操作通常用于**避免阻塞主线程（如 UI 线程或网络请求）**。

---

#### **3. 两者的区别与联系**

- **共同点**  
  两者都涉及“协调”（如线程间的协调或任务执行的协调）。

- **关键区别**  

  | 场景                       | 同步（Synchronization）  | 异步（Asynchrony）       |
  | -------------------------- | ------------------------ | ------------------------ |
  | 线程安全（`StringBuffer`） | 通过锁机制协调多线程访问 | 不适用                   |
  | 代码执行模式               | 阻塞等待操作完成         | 非阻塞，通过回调处理结果 |

- **总结**  
  `StringBuffer` 的“同步”是 **线程安全的实现机制**，而一般操作中的“同步/异步”是 **代码执行模式**。虽然术语相同，但解决的问题不同。

---

#### **4. 为什么需要区分？**

- **性能取舍**  
  `StringBuffer` 的同步会带来性能开销（获取/释放锁），因此在单线程环境下应使用更快的 `StringBuilder`。  
  **示例**：在单线程循环中拼接字符串时，`StringBuilder` 性能显著优于 `StringBuffer`。

- **应用场景**  
  - 多线程共享资源 → 需要同步（如 `StringBuffer`）。  
  - 避免阻塞主线程 → 使用异步操作（如网络请求）。

---

#### **5. 类比理解**

- **同步（线程安全）**  
  想象一个会议室（共享资源），每次只允许一个人（线程）进入并修改白板上的内容（数据）。其他人必须等待前一个人出来后才能进入（`synchronized`）。

- **同步（代码执行）**  
  你去餐厅点餐，站在柜台前等待厨师做好菜才能离开（阻塞等待）。

- **异步（代码执行）**  
  你点完餐后去旁边喝咖啡，服务员会在餐准备好后通知你（回调通知）。

---

通过以上分析，可以明确：**`StringBuffer` 的“同步”是线程安全的机制，而一般操作中的“同步/异步”是代码执行模式**。两者虽然名称相同，但解决的问题不同。









//2

### 集合框架

#### List

​	Java 集合框架主要包括两种类型的容器，一种是**集合（Collection）**，存储一个元素集合，另一种是**图（Map）**，存储键/值对映射。

​	Collection 接口又有 3 种子类型，List、Set 和 Queue，再下面是一些抽象类，最后是具体实现类，常用的有 ArrayList、LinkedList、HashSet、LinkedHashSet、[HashMap](https://www.runoob.com/java/java-hashmap.html)、LinkedHashMap 等等。



#### ArrayList

​	ArrayList是一个**动态数组**，也是我们最常用的集合，是List类的典型实现。

它允许任何符合规则的元素插入甚至包括null，每一个ArrayList都有一个**初始容量（10）**，该容量代表了数组的大小。

随着容器中的元素不断增加，容器的大小也会随着增加，在每次向容器中增加元素的同时都会进行容量检查，当快溢出时，就会进行扩容操作。

所以如果我们明确所插入元素的多少，最好指定一个初始容量值，避免过多的进行扩容操作而浪费时间、效率。

集合框架**是一个用来代表和操纵集合的统一架构**。



![image-20250305210903421](C:\Users\pqy\AppData\Roaming\Typora\typora-user-images\image-20250305210903421.png)

#### Vector

​	与ArrayList相似，但是Vector是**同步**的，**它的操作与ArrayList几乎一样,线程安全的原理是把所有的方法都加上`synchronized`**



#### LinkedList

​	LinkedList是采用**双向循环链表**实现，LinkedList是List接口的另一个实现，除了可以根据索引访问集合元素外，LinkedList还实现了Deque接口，可以当作**双端队列**来使用，也就是说，既可以当作“栈”使用，又可以当作队列使用。

#### 三者对比

1）ArrayList
优点: 底层数据结构是数组，查询快，增删慢。
缺点: **线程不安全**，效率高

2）Vector
优点: 底层数据结构是数组，查询快，增删慢。
缺点: **线程安全**，效率低

3）LinkedList
优点: 底层数据结构是**链表**，查询慢，增删快。
缺点: **线程不安全**，效率高





#### Set

<img src="C:\Users\pqy\AppData\Roaming\Typora\typora-user-images\image-20250305211711644.png" alt="image-20250305211711644" style="zoom:67%;" />

HashSet

​	哈希表实现，元素无序且唯一，**线程不安全**，效率高，可以存储null元素，元素的唯一性是靠**所存储元素类型是否重写hashCode()和equals()方法来保证的**，如果没有重写这两个方法，则无法保证元素的唯一性

LinkedHashSet

​	**链表和哈希表共同实现**，链表保证了元素的顺序与存储顺序一致，哈希表保证了元素的唯一性

TreeSet

​	底层数据结构采用**二叉树**来实现，元素唯一且已经排好序,唯一性同样需要重写hashCode和equals()方法，二叉树结构保证了元素的有序性

Java Set总结

1）HashSet

底层其实是包装了一个**HashMap**实现的
底层数据结构是数组+链表 + 红黑树
具有**比较好的读取和查找性能**， 可以有null 值
通过equals和HashCode来判断两个元素是否相等
**非线程安全**

2）LinkedHashSet

继承HashSet，本质是LinkedHashMap实现
底层数据结构由哈希表(是一个元素为链表的数组)和双向链表组成。
有序的，根据HashCode的值来决定元素的存储位置，同时使用一个链表来维护元素的插入顺序
**非线程安全，可以有null 值**

3）TreeSet

​	是一种排序的Set集合，实现了SortedSet接口，底层是用TreeMap实现的，**本质上是一个红黑树原理**
排序分两种：**自然排序**（存储元素实现Comparable接口）和**定制排序**（创建TreeSet时，传递一个自己实现的Comparator对象）
正常情况下不能有null值，可以重写Comparable接口就可以有null值了。

#### Queue

<img src="C:\Users\pqy\AppData\Roaming\Typora\typora-user-images\image-20250305212351165.png" alt="image-20250305212351165" style="zoom:67%;" />

#### PriorityQueue

​	**PriorityQueue**保存队列元素的顺序并不是按照加入的顺序，而是按照**队列元素的大小**进行排序的。
PriorityQueue不允许插入null元素

#### Deque

Deque接口是Queue接口的子接口，它代表一个双端队列,当程序中需要使用“栈”这种数据结构时，推荐使用**ArrayDeque**





#### Map

<img src="C:\Users\pqy\AppData\Roaming\Typora\typora-user-images\image-20250305213002919.png" alt="image-20250305213002919" style="zoom:67%;" />

#### **1.HashMap**

Map接口基于哈希表的实现，是使用频率最高的用于键值对处理的数据类型。

它根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，特点是**访问速度快，遍历顺序不确定，线程不安全**，最多允许一个key为null，允许多个value为null。

可以用 Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap类。



**扩容**两个参数:

​		初始容量大小和加载因子，初始容量大小是创建时给数组分配的容量大小，**默认值为16**，加载因子默认0.75(**0.75是对空间和时间效率的一种平衡选择**)，用**数组容量大小乘以加载因子**得到一个值，一旦数组中存储的元素个数超过该值就会调用rehash方法将数组容量增加到原来的两倍，专业术语叫做扩容.

​		在做扩容的时候会生成一个新的数组，原来的所有数据需要重新计算哈希码值重新分配到新的数组，所以扩容的操作非常消耗性能.



HashMap 的加载因子（load factor，直译为加载因子，意译为负载因子）是指哈希表中填充元素的个数与桶的数量的比值，当元素个数达到负载因子与桶的数量的乘积时，就需要进行扩容。**这个值一般选择 0.75**，是因为这个值可以在时间和空间成本之间做到一个折中，使得哈希表的性能达到较好的表现。

这就意味着：

- 加载因子越小，填满的数据就越少，哈希冲突的几率就减少了，但**浪费了空间**，而且还会提高扩容的触发几率；
- 加载因子越大，填满的数据就越多，空间利用率就高，但**哈希冲突的几率**就变大了。

**泊松分布有关**

0.693

考虑到 HashMap 的容量有一个要求：它必须是 2 的 n 次幂。当加载因子选择了 0.75 就可以保证它与容量的乘积为整数。





**链表长度>8并且数组长度大于64**，才将链表转换成**红黑树**，变成红黑树的目的是提高搜索速度，高效查询





解决Hash冲突方法有：开放定址法、再哈希法、链地址法（HashMap中常见的**拉链法**）

- 开放定址法
- 再哈希法






#### **2.Hashtable**

​	Hashtable和HashMap从存储结构和实现来讲有很多相似之处，不同的是它承自Dictionary类，而且是**线程安全**的，另外Hashtable不允许key和value为null，并发性不如**ConcurrentHashMap**。

​	Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用**ConcurrentHashMap**替换。



#### **3.LinkedHashMap**

​	LinkedHashMap继承了HashMap，是Map接口的哈希表和链接列表实现，它维护着一个**双重链接列表**，此链接列表定义了迭代顺序，该迭代顺序可以是插入顺序或者是访问顺序。



#### **4.TreeMap**

​	TreeMap实现SortMap接口，能够把它**保存的记录根据键排序，默认是按键值的升序排序（自然顺序）**，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。

![image-20250305213101914](C:\Users\pqy\AppData\Roaming\Typora\typora-user-images\image-20250305213101914.png)

#### 常用对比

**HashMap和Hashtable**:

1.**底层数据结构不同:jdk1.7底层都是数组+链表,但jdk1.8 HashMap加入了红黑树**
2.Hashtable 是不允许键或值为 null 的，HashMap 的键值则都可以为 null。
3.添加key-value的hash值算法不同：HashMap添加元素时，是使用自定义的哈希算法,而HashTable是直接采用**key的hashCode()**
4.同步性不同: **Hashtable是同步(synchronized)的，**适用于多线程环境,而hashmap不是同步的，适用于单线程环境。

多个线程可以共享一个Hashtable；而如果没有正确的同步的话，多个线程是不能共享HashMap的。



**HashMap和ConcurrentHashMap**:

​	都是采用数组+链表或者数组+红黑树的方式实现。这二者底层数据结构都是以数组为主体的。

线程安全：**HashMap是线程不安全的，ConcurrentHashMap是线程安全的**。

