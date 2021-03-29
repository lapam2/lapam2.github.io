---
title: HashMap源码分析
date: 2021-03-26 11:26:17
tags:
typora-root-url: ..
---

---

## Map接口实现类比较 ：

Iterable 接口 有Collection接口和Map接口 两个子接口。java.util.Collection 和 java.util.Map。java.lang.Iterable

Map接口 继承 Iterable 接口。Map接口有  HashMap实现类，LinkedHashMap实现类，TreeMap实现类。

## HashMap底层实现：

![image-20210329110540547](/pic/image-20210329110540547.png)

![image-20210329111217372](/pic/image-20210329111217372.png)

![image-20210329111357041](/pic/image-20210329111357041.png)

![image-20210329141141860](/pic/image-20210329141141860.png)

![image-20210329141546541](/pic/image-20210329141546541.png)

![image-20210329141703542](/pic/image-20210329141703542.png)

![image-20210329141747601](/pic/image-20210329141747601.png)

![image-20210329141823983](/pic/image-20210329141823983.png)

![image-20210329141902811](/pic/image-20210329141902811.png)



## 源码分析：

```java
HashMap map = new HashMap();
map.put("aava",10);//ok
map.put("bhp",10);//ok
map.put("cana",20);//替换value
System.out.println("map="+map);
```



## LinkedHashMap底层实现：

## 源码分析：

```java

```



## TreeMap底层实现：

![image-20210329142221408](/pic/image-20210329142221408.png)

![image-20210329142432906](/pic/image-20210329142432906.png)

![image-20210329143012799](/pic/image-20210329143012799.png)

![image-20210329143046638](/pic/image-20210329143046638.png)

![image-20210329143151782](/pic/image-20210329143151782.png)



## 源码分析：

```java

```



## 常见问题：

1. **HashMap 和 Hashtable区别？**
   1. HashMap 是将键映射到值的对象，key:value都是对象。不能包括重复键，可以包括重复值。但可以存在一个null key ,多个null value。线程不安全，但效率高于Hashtable。HashMap 去掉Hashtable 里的contains方法，添加了containsKey，containsValue方法。
   2. Hashtable不能包括null key 和null value。线程安全的，方法中有Synchronize，在多个线程访问Hashtable时，不需要再为自己的方法实现同步。
2. **HashMap 底层结构？**
   1. HashMap由数组+链表组成。数组是主体，链表是为解决hash冲突存在。
3. **HashSet 实现原理？**
   1. 底层实现是基于HashMap,（数组+链表+红黑树）
4. **HashMap 怎样解决hash冲突？**
   1. 链表法
5. **HashMap 为什么线程不安全？**
6. **如何决定使用HashMap 和 TreeMap?**
   1. 无序：使用HashMap
   2. 排序：使用TreeMap



## Collections工具类

![image-20210329143556814](/pic/image-20210329143556814.png)

![image-20210329143915970](/pic/image-20210329143915970.png)

![image-20210329144117828](/pic/image-20210329144117828.png)

![image-20210329144310987](/pic/image-20210329144310987.png)

![image-20210329144650580](/pic/image-20210329144650580.png)

![image-20210329144911114](/pic/image-20210329144911114.png)

![image-20210329145215217](/pic/image-20210329145215217.png)

![image-20210329145311297](/pic/image-20210329145311297.png)

![image-20210329145452023](/pic/image-20210329145452023.png)

![image-20210329145513994](/pic/image-20210329145513994.png)

![image-20210329150040879](/pic/image-20210329150040879.png)

![image-20210329151242269](/pic/image-20210329151242269.png)

![image-20210329154211076](/pic/image-20210329154211076.png)

![image-20210329182242287](/pic/image-20210329182242287.png)

![image-20210329185302827](/pic/image-20210329185302827.png)

![image-20210329185731593](/pic/image-20210329185731593.png)

![image-20210329190209871](/pic/image-20210329190209871.png)

![image-20210329190342941](/pic/image-20210329190342941.png)

![image-20210329190947283](/pic/image-20210329190947283.png)

![image-20210329191340209](/pic/image-20210329191340209.png)