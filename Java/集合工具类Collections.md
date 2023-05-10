+ Collections不是集合，而是集合的工作类。

+ 常见的API

  | 方法名称                                     | 说明                            |
  | -------------------------------------------- | :------------------------------ |
  | boolean addALL(Collection<T> c,T...elements) | 向单列集合批量添加元素          |
  | void shuffle(List<?> list)                   | 打乱list集合元素的顺序          |
  | void sort(List<T> list)                      | 排序                            |
  | void sort(List<T> list,Comparator<T> c)      | 根据指定规则进行排序            |
  | int binarySearch(List<T>, T key)             | 以二分查找法查找元素            |
  | void copy(List<T> dest,List<T> src)          | 拷贝集合中的元素                |
  | int fill(List<T> list,T obj)                 | 使用指定的元素填充集合          |
  | void max/min(Collection<T> coll)             | 根据默认的自然排序获取最大/小值 |
  | void swap(List<?> list,int i,int j)          | 交换集合中指定位置的元素        |

  