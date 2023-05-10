![](C:\gitcode\the-notes-of-itheima\Image\Map.png)

红色为接口，蓝色为实现类

Map是双列集合的顶层接口，它的功能是全部双列集合都可以继承使用的

| 方法名称                     | 说明                                 |
| :--------------------------- | :----------------------------------- |
| put(key,value)               | 添加元素                             |
| remove(key)                  | 根据键删除键值对元素                 |
| void clear()                 | 移除所有的键值对元素                 |
| boolean containsKey(key)     | 判断集合是否包含指定的键             |
| boolean containsValue(value) | 判断集合是否包含指定的值             |
| boolean isEmpty()            | 判断集合是否为空                     |
| int size()                   | 集合的长度，也就是集合中键值对的个数 |



#### Map集合的第一种遍历方式（键找值）

+ 涉及到的方法：
  + Map实现类 . keySet() 获取到Map集合里面所有的键，返回Set集合类型。
  + Map实现类 . get(键) 获取键所对应的值。

```java
public class test {
    public static void main(String[] args) {
//1.创建Map集合的实现类
        Map<String,String>map = new HashMap<>();//Map作为一个接口，需要一个实现类，HashMap就是Map的实现类
//2.添加元素
        map.put("妻子1","老公1");
        map.put("妻子2","老公2");
        map.put("妻子3","老公3");
//3.通过键去找对应的值
        //3.1 把获取到的键先存到单列集合里面
        Set<String>keys = map.keySet();
        //3.2 通过遍历单列集合，利用get方法获得键所对应的值
        for(String key : keys){//通过加强for去遍历Set集合
            String value = map.get(keys);//变量value存的就是获取到的值
            sout(key + "=" + value);
        }
    }
}
```

#### Map集合的第二种遍历方式（键值对）

+ 涉及到的方法：
  + 键值对对象 . getkey() 获取实现类所有的键。
  + 键值对对象 . getvalue() 获取实现类所有的值。
  + Map实现类 . entrySet() 获取所有的键值对对象。返回一个Set集合，Set的泛型为键值对对象Entry(String,String)

```java
import java.util.Map.Entry
public class test {
    public static void main(String[] args) {
      //1.创建Map集合的实现类
        Map<String,String>map = new HashMap<>();//Map作为一个接口，需要一个实现类，HashMap就是Map的实现类
      //2.添加元素
        map.put("妻子1","老公1");
        map.put("妻子2","老公2");
        map.put("妻子3","老公3");
      //3. 通过键值对对象进行遍历
        //3.1 通过entrySet方法获取键值对对象并存储在Set集合里面
        Set<Entry<String,String>> entries= map.entrySet();
        //3.2 遍历Set集合，通过getkey,getvalue方法获取键值对对象对应的键和值
        for(Entry<String,String>entry : entries){//通过加强for的方法对Set集合进行遍历，变量entry表示获取的键值对对象
            String key = entry.getkey();
            String value = entry.getvalue();
        }    
    }
}
```

+ Entry是Map的内部类

#### Map集合的第三种遍历方式（lambda表达式）

+ 涉及到的方法：

|                           方法名称                           |         说明          |
| :----------------------------------------------------------: | :-------------------: |
| default void forEach(BiConsumer<? super K, ? super V> action) | 结合lambda遍历Map集合 |

  

```java
import java.util.HashMap;
import java.util.Set;
import java.util.Map.Entry;
import java.util.Map;
import java.util.function.BiConsumer;

public class test {
    public static void main(String[] args) {
        //1.创建Map集合的实现类
        Map<String,String> map = new HashMap<>();
        //2.添加元素
        map.put("妻子1", "老公1");
        map.put("妻子2", "老公2");
        map.put("妻子3", "老公3");
        //3.利用lambda表达式进行遍历
        map.forEach((String key, String value)->System.out.println(key + "=" + value));
    }
}
```

匿名内部类源码

```java
        map.forEach(new BiConsumer<String, String>() {
            @Override
            public void accept(String key, String value) {
                System.out.println(key + "=" + value);
            }
```
+ BiConsumer是一个函数式接口

#### HashMap的基本使用

1. HashMap底层是哈希表结构的

2. 依赖hashCode方法和equals方法保证键的唯一

3. 如果键存储的是自定义对象，需要重写hashCode方法和equals方法

   如果值存储的是自定义对象，不需要重写hashCode方法和equals方法
