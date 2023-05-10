1. 特点

  + 红黑树结构，增删改查性能好

  + 由键决定特性：不重复、无索引、可排序

  + 可排序：对键进行排序

  + 默认按照键从小到大进行排序，也可以自己规定键的排序规则

2. 两种排序规则

   + 实现Comparable接口，指定比较规则。

   + 创建集合时传递Comparator比较器对象，指定比较规则。

+ 当键是非自定义类型时：

```java
public class test {
    public static void main(String[] args) {
        TreeMap<Integer,String> tm=new TreeMap<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                //o1是要添加的元素
                //o2是已经添加在红黑树中的元素
                return o2-o1;//降序
            }
        });
        tm.put(1,"奥利奥");
        tm.put(2,"aoligei");
        tm.put(3,"aoligei2");
        System.out.println(tm);
    }
}
```

+ 当键是自定义对象时：
```java
public class test {
    public static void main(String[] args) {
        TreeMap<Student,String> tm=new TreeMap<>();
        Student s1=new Student("zhangsan",23);
        Student s2=new Student("wangwu",25);
        Student s3=new Student("lisi",24);
        tm.put(s1,"天津");
        tm.put(s2,"江苏");
        tm.put(s3,"北京");
        System.out.println(tm);
    }
}

public class Student implements Comparable<Student>{
    private String name;
    private int age;
 @Override
    public int compareTo(Student o) {
      int i = this.getAge()-o.getAge();
      i = (i == 0) ? (this.getName().compareTo(o.getName())) : i;
      return i;
    }
}
```

  

```java
public class test {
    public static void main(String[] args) {
        String s = "adasdsadsadbc";
        TreeMap<Character,Integer> tm = new TreeMap<>();
        for(int i = 0;i < s.length();i++){
            char c = s.charAt(i);
            if(tm.containsKey(c)){
                int count = tm.get(c);
                count++;
                tm.put(c,count);
            }else {
                tm.put(c,1);
            }
        }
        StringBuilder sb = new StringBuilder();
        tm.forEach(new BiConsumer<Character, Integer>() {
            @Override
            public void accept(Character key, Integer value) {
                sb.append(key).append("(").append(value).append(")");
            }
        });
        System.out.println(sb);
    }
}

```

