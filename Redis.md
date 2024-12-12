# Redis
  
Remote Dictionary Service
  
## 1 基础数据结构

### 1.1 string 字符串
  
1.1.1
![image](https://github.com/user-attachments/assets/642390a3-6409-4147-b594-0e1b03b66e7f)
  
1.1.2 Redis的字符串是动态字符串，是可以修改的字符串，内部结构是线上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配，内部为当前字符串实际分配的空间capacity一般要高于实际字符串长度len。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。字符串最大长度为512M。
  
1.1.3
![image](https://github.com/user-attachments/assets/4743870e-5c81-4279-b4e9-3e4bb3500f09)

1.1.4 如果value值是一个整数，还可以对它进行自增操作。自增的范围是signed long的最大最小值。

### 1.2 list 列表

1.2.1 Redis的列表相当于Java语言里的LinkedList，插入和删除的时间复杂度为O(1)，索引定位的时间复杂度为O(n)。

1.2.2 当列表弹出了最后一个元素之后，该数据结构自动被删除，内存被回收。

1.2.3
![image](https://github.com/user-attachments/assets/32cb451a-0d31-4e58-891a-8b659bc4ff53)

1.2.4 ziplist即压缩列表，将所有的元素紧挨着一起存储，分配的是一块连续的内存。当数据量比较多的时候才会改成quicklist。因为普通的链表需要的附加指针空间太大，会比较浪费空间，而且会加重内存的碎片化。Redis将链表和ziplist结合起来组成了quicklist。将多个ziplist使用双向指针串起来使用，既满足了快速的插入删除性能，又不会出现太大的空间冗余。

### 1.3 hash 哈希

1.3.1 Redis的字典相当于Java语言里面的HashMap，它是无序字典。内部实现结构为数组+链表二维结构

1.3.2
![image](https://github.com/user-attachments/assets/ac04966a-7325-4ac4-902e-472a9249e141)

1.3.3 渐进式rehash会在rehash的同时，保留新旧两个hash结构，查询时会同时查询两个hash结构，然后在后续的定时任务中以及hash的子指令中，循序渐进地将旧hash的内容一点点迁移到新的hash结构中。

1.3.4
![image](https://github.com/user-attachments/assets/bea0da6b-8818-42c6-8773-7d8868ecd7e9)

### 1.4 set 集合

1.4.1 Redis的集合相当于Java语言里面的HashSet，它内部的键值对是无序的唯一的。它的内部实现相当于一个特殊的字典，字典中的所有的value都是一个值NULL。

### 1.5 zset 有序集合
