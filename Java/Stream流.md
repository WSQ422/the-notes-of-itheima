#### 作用

结合了Lambda表达式，简化集合、数组的草操作。

#### 使用步骤

1. 先得到一条Stream流，并把数据放上去。

   | 获取方式     | 方法名                                  | 说明                       |
   | ------------ | --------------------------------------- | -------------------------- |
   | 单列集合     | default Stream<E> stream()              | Collection接口中的默认方法 |
   | 双列集合     | 无                                      | 无法直接使用Stream流       |
   | 数组         | static <T> Stream <T> stream(T[] array) | Arrays工具类中的静态方法   |
   | 一堆零散数据 | static <T> Stream <T> of(T...values)    | Stream接口中的静态方法     |

   

2. 利用Stream流中的API进行各种操作

   > 中间方法：过滤  转换
   >
   > 终结方法：统计  打印

+ 单列集合

```java
public class test {
    public static void main(String[] args) {
         ArrayList <String> list = new ArrayList<>();                            Collections.addAll(list,"a","b","c","d","e");
        Stream<String> stream1 = list.stream();
        stream1.forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                //s依次表示流水线上的每一个数据
                System.out.println(s);
            }
        });
    }
}
```

+ 双列集合

```java
public class test {
    public static void main(String[] args) {
        HashMap<String,Integer> hm = new HashMap<>();
        hm.put("aaa",111);
        hm.put("bbb",222);
        hm.put("ccc",333);
        //第一种获取Stream流的方法
 hm.keySet().stream().forEach(s-> System.out.println(s));
        //第二种获取Stream流的方法
 hm.entrySet().stream().forEach(s-> System.out.println(s));
    }
}
```

+ 数组

```java
public class test {
    public static void main(String[] args) {
        int[]arr = {1,2,3,4};
Arrays.stream(arr).forEach(s-> System.out.println(s));
    }
}
```

+ 一堆零散数据

```java
public class test {
    public static void main(String[] args) {
      Stream.of(1,2,3,4).forEach(s-> System.out.println(s));
    }
}
```

+ 注意：Stream接口中的静态方法of的形参是一个可变参数，可以传递一堆零散的数据，也可以传递数组（可变参数本质上就是一个数组），但是传递数组时必须是引用数据类型的数组，如果传递基本数据类型的数组，方法会把整个数组当作一个元素放在Stream流中。

#### Stream流的中间方法

| 名称                                    | 说明                                   |
| --------------------------------------- | -------------------------------------- |
| filter(Predicate<? super T> predicate)  | 过滤                                   |
| limit(long maxSize)                     | 获取前几个元素                         |
| skip(long n)                            | 跳过前几个元素                         |
| distinct()                              | 元素去重，依赖（hashCode和equals方法） |
| static Stream concat(Stream a,Stream b) | 合并a和b两个流为一个大流               |
| map(Function<T,R> mapper)               | 转换流中的数据类型                     |

+ 中间方法返回的是新的Stream流，原来的Stream流只能用一次，建议使用链式编程。
+ 修改Stream流中的数据，不会影响原来集合或者数组中的数据。
