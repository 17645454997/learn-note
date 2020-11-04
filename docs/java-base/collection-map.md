## Java集合类
 * Map 接口和 Collection 接口是所有集合框架的父接口：
 * Collection 接口的子接口包括：Set 接口和 List 接口 
 * Map 接 口 的 实 现 类 主 要 有 ： HashMap 、 TreeMap 、 Hashtable 、 ConcurrentHashMap 以及 Properties 等 
 * Set 接口的实现类主要有：HashSet、TreeSet、LinkedHashSet 等 
 * List 接口的实现类主要有：ArrayList、LinkedList、Stack 以及 Vector 等
* **HashMap 与 HashTable 的区别**
* HashMap 没有考虑同步，是线程不安全的；
* Hashtable 使用了 synchronized 关键字，是线程安全的；
* HashMap 允许 K/V 都为 null；后者 K/V 都不允许为 null；
* HashMap 继承自 AbstractMap 类；
* 而 Hashtable 继承自 Dictionary 类；
* **哈希冲突和解决办法**
* 什么是哈希冲突？
* 当两个不同的输入值，根据同一散列函数计算出相同的散列值的现象，我们就把它叫做碰撞（哈 希碰撞）。
* 开放定址法（线性探测再散列，二次探测再散列，伪随机探测再散列）
* 再哈希法
* 链地址法(Java hashmap就是这么做的)
* 建立一个公共溢出区，建立一个公共溢出区域，就是把冲突的都放在另一个地方，不在表里面。

* **快速失败和安全失败**
* 快速失败：在使用迭代器对集合对象进行遍历的时候，如果 A 线程正在对集合进行遍历， 此时 B 线程对集合进行修改（增加、删除、修改），或者 A 线程在遍历过程 中对集合进行修改，都会导致 A 线程抛出 ConcurrentModificationException 异 常。
* 安全失败：采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是先 复制原有集合内容，在拷贝的集合上进行遍历。 由于迭代时是对原集合的拷贝进行遍历，所以在遍历过程中对原集合所作的修改 并不能被迭代器检测到，故不会抛 ConcurrentModificationException 异常