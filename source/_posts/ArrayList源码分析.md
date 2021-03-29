---
title: ArrayList源码分析
date: 2021-03-25 23:00:50
tags:
---

---

## List接口中实现类对比：

 均在java.util包里，Collection接口 继承 Iterable 接口。List 接口 继承 Collection接口，Set 接口 继承 Collection接口。List 接口 有Vector、ArrayList、LinkedList 三个实现类。

- Vector类：

  可变数组（扩容机制），版本jdk1.1, 安全，效率不高，

  扩容倍数--> 如果是无参，默认为10， 满后，扩容2倍；如有参，按指定大小扩容2倍。

- ArrayList类 :

  可变数组（扩容机制），版本jdk1.2, 不安全，效率高，

  扩容倍数-->如果无参构造，初始为0，第一次添加为10，第二次扩1.5倍。如果有参构造，1.5倍；

- LinkedList类 :

  基于双向链表的节点构成，没有扩容机制，可随时增减。

- 如何选择？

  改查多，选ArrayList；增删多，选LinkedList；程序中大部分都是查询，大部分情况都是选ArrayList；项目中根据业务灵活选择。

<!--more-->

## ArrayList底层实现：

若调用无参构造，初始elementData容量为0，第一次添加，则扩容elementData容量为10，如再需要扩容，扩容elementData为1.5倍；

若调用有参构造，初始elementData容量为指定大小，需要扩容，则直接扩容elementData为1.5倍。

## 源码分析：

```java
//新建ArrayList对象，调用无参构造器
List list = new ArrayList();
#调用无参构造，默认是一个空的elementData
transient Object[] elementData;//临时的,暂时的，elementData是一个对象数组
public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
//调用add()方法
public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
}
//调用确定容量区间方法，
private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }
//调用确定明确的容量
 private static int calculateCapacity(Object[] elementData, int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
//真正的扩容机制，调用grow()方法，扩为1.5倍
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

//新建ArrayList对象，调用有参构造器，确定了初始容量。后续add()和前文一样
List list = new ArrayList(3);
public ArrayList(int initialCapacity) { 
     this.elementData = new Object[initialCapacity];   
}


```



## Vector底层实现：

若调用无参构造，初始elementData容量为10，如再需要扩容，扩容elementData为2倍；

若调用有参构造，初始elementData容量为指定大小，需要扩容，则直接扩容elementData为2倍。

## 源码分析：

```java
 // 新建 Vector 对象，调用无参构造器
        Collection list = new Vector();
// 初始容量为10
 public Vector() {
        this(10);
    }
// 调用add()方法，有synchronized关键字，线程同步，故线程安全。
 public synchronized boolean add(E e) {
        modCount++;
        ensureCapacityHelper(elementCount + 1);
        elementData[elementCount++] = e;
        return true;
    }
// 确定容量
private void ensureCapacityHelper(int minCapacity) {
        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
// 真正的扩容机制，调用grow()方法，扩为2倍
 private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

```

## LinkedList底层实现：

底层是基于链表实现。

```java
// 基于双向链表的节点构成
private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```



## 源码分析：

```java
// 新建 LinkedList 对象，调用无参构造器
        Collection list = new LinkedList();
// 构建一个空的list
  public LinkedList() {
    }
// 调用add()方法，
 public boolean add(E e) {
        linkLast(e);
        return true;
    }
//addLast()方法和add()方法一样
 public void addLast(E e) {
        linkLast(e);
    }
// 在list末尾添加一个Node
   void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }
// LinkedList 底层是链表的数据结构，容量不用事先定好，故没有扩容机制，可随时增减。
```



## 常见问题：

1. **List，Set ,Map 之间的区别？**
   1. List列表：线性存储，可以存放重复对象，主要有两个实现类ArrayList 和LinkedList。ArrayList 是长度可变的数组，查改速度快。扩容机制通过grow()方法使用1.5倍计算扩容容量，调用Arrays.copyof()方法对原数组进行复制。LinkedList采用链表数据结构，插入和删除速度快。
   2. Set集合：Set中对象不安特定方式排序，没有重复对象。主要有两个实现类HashSet 和TreeSet。HashSet按照哈希算法存取集合中的对象，存取速度快。扩容机制是当元素个数超过数组大小的0.75倍时，就会进行两倍扩容。TreeSet实现了SortedSet接口，能对集合中对象排序。
   3. Map映射：是把键对象和值对象映射的集合，每一个元素都包括一个键对象和值对象。主要有三个实现类HashMap 和LinkedHashMap 和TreeMap。HashMap基于散列表实现。LinkedHashMap类似于HashMap，但迭代遍历时取的<K,V>顺序是其插入顺序。TreeMap 基于红黑树实现，查看<K,V>时会被排序，TreeMap是唯一带有subMap()方法的Map, 可以返回一个子树。
2. **ArrayList 和LinkedList 区别？**
   1. ArrayList实现基于动态数组的数据结构。LinkedList 基于链表的数据结构。对于查改操作，ArrayList的get和set方法优于LinkedList。对于增删操作，LinkedList的add和remove方法优于ArrayList。
3. **ArrayList 和 Vector区别？**
   1. Vector是线程安全的，他的方法之间是线程同步的。ArrayList是线程不安全的，方法之间是线程不同步的。如果只有一个线程访问集合，推荐使用ArrayList，他不考虑线程安全，效率较高。如果多个线程访问集合，使用Vector。
   2. 扩容机制不同，ArrayList默认初始容量为0，增加一个时容量扩为10，超过时扩为原来容量的1.5倍。Vector扩为原来容量的2倍。
4. **Array 和 ArrayList区别？**
   1. ArrayList只能包括引用类型。基于数组实现，通过底层扩容机制可以自动调整容量。
   2. Array包括基本数据类型和引用类型。大小不可以调整。