---
layout: post
title:  "HashMap and HashTable"
description: "HashMap and HashTable"
date:   2016-10-13 21:19:46 +0800
categories:
tag: java
---
一、HashMap and HashTable Difference
1.Synchronization(Thread Safe)
当不需要多线程任务操作，可以选择HashMap, 效率高；
HashTable是线程安全，同步的

2.Null keys and values
HashMap允许one null key and multi-null values
HashTable不允许

3.Iterating values
HashMap使用iterator遍历，HashTable是除了vector外，唯一的使用enumerator遍历的

4.Fail fast iterator
HashMap: fail-fast iterator
HashTable: fail-save iterator
如果iterator created,改变hash table结构，(除了本身的remove)
会报ConcurrentModification Exception
5.Performance
在单线程任务的环境下，HashMap性能更好，而且利用更少的memory

6.Superclass and Legacy
Hashtable is subclass of Dictionary class which is now obsolete in jdk1.7
alternative way:
1.synchronizing a hashMap
2.use a ConcurrentMap implementation(e.g ConcurrentHashMap)

HashMap is subclass of AbstractMap

Similarities:
1.不保证插入顺序
2.实现Map interface
3.constant time performance for put and get methods
4.works on the principle of Hashing

二、Iterator and Enumeration
iterator is an interface
hasNext, next, remove,  forEachRemaining(jdk1.8)
(allow remove, friendly method name)
Enumeration is an interface
hasMoreElement, nextElement
{% highlight ruby %}
for(int i = 0; i < v.size(); i++) vs while(v.hasNext() || v.hasMoreElements())
{% endhighlight %}
--前者需要更多的操作
iterator on hashmap
{% highlight ruby %}
Iterator iter = hashmap.entrySet().iterator(); //效率高
iterator iter = hashmap.keySet().iterator();   //先取entryset, 再按key取keySet
{% endhighlight %}
foreach vs while
在foreach中删除元素，会有异常（jdk1.5以后引入）

三、HashMap and ConcurrentHashMap
ConcurrentHashMap: thread-safe
lock on Map object: synchronizedMap(HashMap) —> hashtable
ConcurrentHashMap synchronizes or locks on the certain portion of the Map
Map is divided into different partitions depending upon the concurrency level.
ConcurrentHashMap: not allow null values as well as null key
Performance

四、How Hashmap works in Java
Hash Function
Hash Value
Bucket
