######2016-02-26
---
### Hashtable详解  
#### 1 Hashtable介绍  
&nbsp;&nbsp;&nbsp;&nbsp; Hashtable 继承于Dictionary，实现了Map、Cloneable、java.io.Serializable接口。和HashMap一样，Hashtable 也是一个散列表，它存储的内容是键值对(key-value)映射。在Java中散列表用链表数组实现，每个列表被称为桶（bucket）。  
&nbsp;&nbsp;&nbsp;&nbsp;Hashtable的函数都是同步的，这意味着它是线程安全的。它的key、value都不可以为null。此外，Hashtable中的映射不是有序的。  
&nbsp;&nbsp;&nbsp;&nbsp;　Hashtable 的实例有两个参数影响其性能：初始容量 和 加载因子。容量 是哈希表中桶 的数量，初始容量 就是哈希表创建时的容量。注意，哈希表的状态为 open：在发生“哈希冲突”的情况下，单个桶会存储多个条目，这些条目必须按顺序搜索。加载因子 是对哈希表在其容量自动增加之前可以达到多满的一个尺度。初始容量和加载因子这两个参数只是对该实现的提示。关于何时以及是否调用 rehash 方法的具体细节则依赖于该实现。  
&nbsp;&nbsp;&nbsp;&nbsp;通常，默认加载因子是 0.75, 这是在时间和空间成本上寻求一种折衷。加载因子过高虽然减少了空间开销，但同时也增加了查找某个条目的时间(在大多数 Hashtable 操作中，包括 get 和 put 操作，都反映了这一点)。  
##### 1.1 与Hashtable相关的基本概念  
###### 散列码（hash code）  
&nbsp;&nbsp;&nbsp;&nbsp;散列表为每个对象所计算的一个整数；具有不同数据域的对象将产生不同的散列码  
###### 散列冲突（hash collision） 
&nbsp;&nbsp;&nbsp;&nbsp;将元素直接插入桶中时，有时会遇见桶被占满的情况，这种现象被称为散列冲突。当遇到这种情况时就要将新元素与桶中的元素进行比较，看这个对象是否已存在。如果散列码是合理且随机分布的，桶的数目也足够大，需要比较的次数就会少。  
###### 再散列（rehashed）  
&nbsp;&nbsp;&nbsp;&nbsp; 如果散列表太满，就需要再散列，即创建一个桶数更多的表，而装填因子决定了何时对散列表进行再散列。  
##### 2 Hashtable的遍历方式  
##### 2.1 遍历Hashtable的键值对  
###### 第一步：根据entrySet()获取Hashtable的“键值对”的Set集合。  
###### 第二步：通过Iterator迭代器遍历“第一步”得到的集合。  
```java  
// 假设table是Hashtable对象
// table中的key是String类型，value是Integer类型
Integer integ = null;
Iterator iter = table.entrySet().iterator();
while(iter.hasNext()) {
    Map.Entry entry = (Map.Entry)iter.next();
    // 获取key
    key = (String)entry.getKey();
        // 获取value
    integ = (Integer)entry.getValue();
}  
```  
##### 2.2 通过Iterator遍历Hashtable的键  
###### 第一步：根据keySet()获取Hashtable的“键”的Set集合。  
###### 第二步：通过Iterator迭代器遍历“第一步”得到的集合。  
```java  
// 假设table是Hashtable对象
// table中的key是String类型，value是Integer类型
String key = null;
Integer integ = null;
Iterator iter = table.keySet().iterator();
while (iter.hasNext()) {
        // 获取key
    key = (String)iter.next();
        // 根据key，获取value
    integ = (Integer)table.get(key);
}  
```  
##### 2.3 通过Iterator遍历Hashtable的值  
###### 第一步：根据value()获取Hashtable的“值”的集合。
###### 第二步：通过Iterator迭代器遍历“第一步”得到的集合。  
```java  
// 假设table是Hashtable对象
// table中的key是String类型，value是Integer类型
Integer value = null;
Collection c = table.values();
Iterator iter= c.iterator();
while (iter.hasNext()) {
    value = (Integer)iter.next();
}  
```  
##### 2.4 通过Enumeration遍历Hashtable的键  
###### 第一步：根据keys()获取Hashtable的集合。  
###### 第二步：通过Enumeration遍历“第一步”得到的集合。  
```java  
Enumeration enu = table.keys();
while(enu.hasMoreElements()) {
    System.out.println(enu.nextElement());
}    
```  
##### 2.5 通过Enumeration遍历Hashtable的值  
###### 第一步：根据elements()获取Hashtable的集合。  
###### 第二步：通过Enumeration遍历“第一步”得到的集合。  
```java  
Enumeration enu = table.elements();
while(enu.hasMoreElements()) {
    System.out.println(enu.nextElement());
}  
```  

