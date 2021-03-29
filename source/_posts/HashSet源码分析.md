---
title: HashSet源码分析
date: 2021-03-26 11:18:52
tags:
typora-root-url: ..
---

---

## Set接口实现类比较 ：

Collection接口 继承 Iterable 接口。List 接口 和Set接口是  Collection接口的两个子接口。Set接口有  HashSet实现类，LinkedHashSet实现类，TreeSet实现类。

## HashSet底层实现：

1. HashSet 底层是HashMap：数组+链表+红黑树

2. 添加一个元素时，先得到hash值，--会转成-->索引值

3. 找到存储数据表table,看这个索引位置是否已存放元素

4. 如果没有，直接加入

5. 如果有，调用equals()比较。如果相同，放弃添加；如果不相同，则添加到链表最后。

6. 在jdk8中，如果一条链表元素个数有8个或超过8个，且table数组大小有64个，则将该条链表会进行树化（红黑树）

   <!--more-->

   ![image-20210328211921572](/pic/image-20210328211921572.png)

**比较equals()方法，是由程序员制定equals()方法的标准。要具体看重写的equals()方法**。例如：String类重写了equals()方法，比较的就是里面的内容。

## HashSet扩容机制：

![image-20210328212303137](/pic/image-20210328212303137.png)

![image-20210328212752472](/pic/image-20210328212752472.png)

## 转为红黑树机制：(待补充)

## 源码分析：

```java
HashSet set = new HashSet();
set.add("java"); // 到此为止，第1次add()分析完毕，添加成功
set.add("php");  // 到此为止，第2次add()分析完毕，添加成功
set.add("java"); // 到此为止，第3次add()分析完毕，添加失败
set.add(null);
set.add(null);

// 1.执行HashSet();
public HashSet() {
        map = new HashMap<>();
    }
// 2.执行add();
 public boolean add(E e) {
        return map.put(e, PRESENT)==null;//如果put()返回为null,则表示添加元素成功
    }
// 3.执行put();该方法执行hash(key)
  public V put(K key, V value) {// key ="java", value = PRESENT 共享
        return putVal(hash(key), key, value, false, true);
    }
//执行hash(key),得到key 对应的hash值，算法(h = key.hashCode()) ^ (h >>> 16);无符号右移16位
 static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
// 4.执行putVal(); 核心代码
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;// 定义了辅助变量
    	//table 是HashMap的一个数组，类型是Node[]
	    //if语句 表示当前table是null,或者大小是0
    	// 就是table第一次扩容到16。
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;// resize()之后，table已经变为容量为16的数组，值为空
    	//(1)根据key得到的hash值，去计算该key应该存放在table表的哪个索引位置。
    	//并把这个位置的对象，赋给 辅助变量 p
    	//(2)判断p是否为null
    	//(2.1)如果p为null,表示还没有存放元素，就创建一个Node (key="java",value=PRESENT)
    	//(2.2)就放在该位置，tab[i] = newNode(hash, key, value, null)
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            //一个开发技巧提示：在需要局部变量(辅助变量)的时候，再创建
            Node<K,V> e; K k;//辅助变量
            //如果当前索引位置对应的链表的第一个元素 和 准备添加的key的hash值 一样
            //并且满足下面两个条件之一：
            //(1)准备加入的key 和 p指向的Node节点的key是同一个对象
            //(2)或者 p指向的Node节点的key的 equals() 和 准备加入的key比较后相同
            //就不能加入
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            //再判断p是不是一颗红黑树
            //如果是一颗红黑树，就调用putTreeVal(),来进行添加
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            //如果table对应索引位置，已经是一个链表，就使用for循环比较
            //(1)依次和该链表的每一个元素比较后，都不相同，则加入到该链表的最后
            //	注意：在把元素加入到链表后，立即判断，该链表是否已经达到8个节点
            //		如果达到，则调用treeifyBin()对链表进行树化，转为红黑树
            // 	注意：在转为红黑树时，要进行判断，判断条件
            //		if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY(64))
           	//			resize();
            //		如果上面条件成立，即table数组小于64，先进行table扩容
            //		只有条件不成立，才进行红黑树转换
            //(2)依次和该链表的每一个元素比较过程中，若有相同情况，则直接break
            else {
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
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
    	//size 就是每加入一个节点Node(),不管是在数组还是链表上，都算加入。 size++
        if (++size > threshold)
            resize();//扩容机制
        afterNodeInsertion(evict);
        return null;
    }
//调用resize()方法
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
//红黑树转换
final void treeifyBin(Node<K,V>[] tab, int hash) {
        int n, index; Node<K,V> e;
        if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
            resize();
        else if ((e = tab[index = (n - 1) & hash]) != null) {
            TreeNode<K,V> hd = null, tl = null;
            do {
                TreeNode<K,V> p = replacementTreeNode(e, null);
                if (tl == null)
                    hd = p;
                else {
                    p.prev = tl;
                    tl.next = p;
                }
                tl = p;
            } while ((e = e.next) != null);
            if ((tab[index] = hd) != null)
                hd.treeify(tab);
        }
    }
```



## LinkedHashSet底层实现：

1. 在LinkedHashSet维护一个hash表+双向链表，（LinkedHashSet有head和tail

2. 每一个节点有before和after属性，这样可以形成双向链表

3. 在添加一个元素时，先求hash值，再求索引，确定该元素在table数组的位置，然后将添加的元素加入到双向链表（如果已经存在，则不添加（原则上和HashSet一样）

4. ```java
   tail.next = newElement //示意代码
   newElement.pre = tail
   tail = newElement
   ```

5. 这样，遍历LinkedHashSet也能保证遍历顺序 和 插入顺序一致

![image-20210329073433423](/pic/image-20210329073433423.png)



## 源码分析：

```java
//源码分析 
//1.LinkedHashSet加入顺序 和 取出元素/数据顺序一致
//2.LinkedHashSet底层维护的是LinkedHashMap(是HashMap的子类)
//3.LinkedHashSet底层结构(数组table + 双向链表)
//4.添加第一次时，直接将数组table 扩容到16，存放的节点类型是 LinkedHashMap$Entry
//5.数组是 HashMap$Node[] 存放的元素/数据是 LinkedHashMap$Entry类型
/*
// 继承关系在内部类完成
static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }
*/
LinkedHashSet set = new LinkedHashSet();
        set.add(null);
        set.add(null);
        set.add("java");
        set.add("php");
        set.add("java");
        System.out.println("set="+set);
//构造一个LinkedHashSet,初始容量为16，加载因子为0.75
 public LinkedHashSet() {
        super(16, .75f, true);
    }
//添加第一次
 public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
 public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
//后面和HashSet扩容一样
 

```



## TreeSet底层实现：

1. 当我们使用无参构造器，创建TreeSet时，仍然是无序的
2. 我们希望添加的元素，按照字符串大小来排序
3. 使用TreeSet 提供的一个构造器，可以传入一个比较器（匿名内部类），并指定排序规则
4. 简单看看源码



## 源码分析：

```java
/*
//public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
    }
*/
TreeSet set = new TreeSet();
set.add("aava");
set.add("bhp");
set.add("cana");
System.out.println("set="+set);
//调用无参构造器
public TreeSet() {
        this(new TreeMap<E,Object>());
    }
//构造器把传入的比较器对象
TreeSet set = new TreeSet(new Comparator(){
    @Override
    //调用String 的compareTo方法 进行字符串大小比较
    public int compare(Object o1, Object o2){
        return ((String)o1).compareTo((String)o2);
    }
});
//添加数据
set.add("java");
set.add("bhp");
set.add("hana");
//TreeSet(Comparator<? super E> comparator)的有参构造器，定义比较器的比较方式
  public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
    }
//构造器把传入的比较器对象，赋给TreeSet 底层 TreeMap的属性this.comparator
 public TreeMap(Comparator<? super K> comparator) {
        this.comparator = comparator;
    }
//再调用 TreeSet.add("tom"),底层会执行
// split comparator and comparable paths
        Comparator<? super K> cpr = comparator;
        if (cpr != null) {//cpr 就是我们的匿名内部类(对象)
            do {
                parent = t;
                //动态绑定到我们匿名内部类(对象)compare
                cmp = cpr.compare(key, t.key);
                if (cmp < 0)
                    t = t.left;
                else if (cmp > 0)
                    t = t.right;
                else //如果相等，返回0，则这个key就没有加入
                    return t.setValue(value);
            } while (t != null);
        }
        
```







## 集合实现类如何选择？

主要取决于业务操作特点，根据集合实现类特性进行选择。

1. 判断存储类型（一组对象或一组键值对）
2. 一组对象：Collection接口
   1. 允许重复：List
      1. 增删多：LinkedList	(底层维护一个双向链表)
      2. 改查多：ArrayList      (底层维护一个Object类型的可变数组)
   2. 不允许重复：Set
      1. 无序：HashSet	（底层是HashMap，维护一个hash表，即（数组+链表+红黑树）
      2. 排序：TreeSet
      3. 插入和取出顺序一致：LinkedHashSet    (底层维护数组+双向链表)
3. 一组键值对：Map接口
   1. 键无序：HashMap（底层维护 数组+链表+红黑树）
   2. 键排序：TreeMap
   3. 键插入和取出顺序一致：LinkedHashMap
   4. 读取文件：Properties

![image-20210329074926337](/pic/image-20210329074926337.png)