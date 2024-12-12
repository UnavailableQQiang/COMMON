# Redis
  
Remote Dictionary Service
  
## 基础数据结构

### string 字符串
  
![image](https://github.com/user-attachments/assets/642390a3-6409-4147-b594-0e1b03b66e7f)
  
Redis的字符串是动态字符串，是可以修改的字符串，内部结构是线上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配，内部为当前字符串实际分配的空间capacity一般要高于实际字符串长度len。当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容时一次只会多扩1M的空间。字符串最大长度为512M。
  
![image](https://github.com/user-attachments/assets/4743870e-5c81-4279-b4e9-3e4bb3500f09)

如果value值是一个整数，还可以对它进行自增操作。自增的范围是signed long的最大最小值。

### list 列表
### set 集合
### hash 哈希
### zset 有序集合
