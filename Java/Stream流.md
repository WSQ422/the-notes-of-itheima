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

1. filter完整版示例：

```java
public class test {
    public static void main(String[] args) {
        ArrayList<String > list = new ArrayList<>();
        Collections.addAll(list,"111","222","333","444","555");
        list.stream().filter(new Predicate<String>() {
            @Override
            public boolean test(String s) {
                //如果返回值是true，表示当下数据留下
                //如果返回值是false,表示当下数据剔除
                return s.startsWith("1");
            }
        }).forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });
    }
}
```

简化版(lambda表达式)

```java
list.stream().filter(s-> s.startsWith("1")).forEach(s-> System.out.println(s));
```

2. map完整示例：

```java
public class test {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list,"张三-23","李四-24","王五-25","赵六-26");
        /*new Function<String, Integer>泛型中的第一个类型表示流中原本的数据类型，第二个类型表示转换之后的类型
        apply方法的形参s依次表示流中的每一个数据，返回值为转换之后的数据    */
        list.stream().map(new Function<String, Integer>() {
            @Override
            public Integer apply(String s) {
                //split方法括号中表示分隔的符号，并返回一个数组，arr[0]表示分隔符左边的数据，arr[1]表示分隔符右边的数据
                String[] arr= s.split("-");
                String ageString = arr[1];
                //parseInt() 方法用于将字符串参数作为有符号的十进制整数进行解析。
                int age = Integer.parseInt(ageString);
                return age;
            }
            //forEach中的s的类型为转换之后的Integer
        }).forEach(s-> System.out.println(s));
    }
}
```

简化版：

```java
ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list,"张三-23","李四-24","王五-25","赵六-26");
        list.stream().map(s->Integer.parseInt(s.split("-")[1])).forEach(s-> System.out.println(s));
```

#### Stream流的终结方法

| 名称                          | 说明                       |
| ----------------------------- | -------------------------- |
| void froEach(Consumer action) | 遍历                       |
| long count()                  | 统计                       |
| toArray()                     | 收集流中的数据，放到数组中 |
| collect(Collectors collector) | 收集流中的数据，放到集合中 |

完整代码演示：

```java
public class test {
    public static void main(String[] args) {
        ArrayList<String > list = new ArrayList<>();
        Collections.addAll(list,"111","222","333","444","555");
        
        long count = list.stream().count();
        
        Object[] arr1 = list.stream().toArray();
        
        /*
        IntFunction的泛型：具体类型的数组
        apply的形参：流中数据的个数，要跟数组的长度保持一致
        apply的返回值：具体类型的数组
        toArray方法的底层会依次得到流中的每一个数据并把数据放在数组当中
         */
        String[] arr= list.stream().toArray(new IntFunction<String[]>() {
            @Override
            public String[] apply(int value) {
                return new String[value];
            }
        });
    }
}
```

```java
ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list,"张三-男-23","李四-男-24","王五-女-25","赵六-男-26");
        //Collectors是一个实现类，Collectors中的toList方法可以把流中的数据放到一个新的ArrayList中去，并返回这个集合
        List<String> newlist = list.stream().filter(s -> "男".equals(s.split("-")[1])).collect(Collectors.toList());
        System.out.println(arr);
//也可以调用Collectors中的toSet方法，把流中的数据放到一个新的hashSet集合中去并返回这个集合(会自动去除重复元素)
```

```java
public class test {
    public static void main(String[] args) {
         ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list,"张三-男-23","李四-男-24","王五-女-25","赵六-男-26");
        参数一:
                Function泛型一:表示流中每一个数据的类型
                        泛型二:表示Map集合中键的数据类型

                方法apply形参:依次表示流里面的每一个数据
                        方法体:生成键的代码
                        返回值:已经生成的键
          参数二:类似参数一，表示值的生成规则
         */
        Map<String,Integer> map = list.stream().collect(Collectors.toMap(new Function<String, String>() {
            @Override
            public String apply(String s) {
                return s.split("-")[0];
            }
        }, new Function<String,Integer>() {
            @Override
            public Integer apply(String t) {
                return Integer.parseInt(t.split("-")[2]);
            }
        }));
        System.out.println(map);
    }
}//toMap方法可以用lambda表达式简化书写
```

