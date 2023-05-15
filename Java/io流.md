### 概述

用于读写文件中的数据 （本地文件、网络）

+ IO流的分类：

  + 按照流的方向

    > 输入流：读取
    >
    > 输出流：写出
    
  + 按照操作文件类型
  
    > 字节流：所有类型的文件
    >
    > 字符流：纯文本文件

### IO流的体系

#### FileOutputStream(字节输出流)

##### 书写步骤：

  > 1 创建字节输出流对象
  >
  > 2 写入数据
  >
  > 3 释放资源

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        //第一步
        FileOutputStream fos = new FileOutputStream("myio\\a.txt");
        //第二步
        fos.write(100);
        //第三步
        fos.close();
    }
}
```

+ 注意：
  +  new FileOutputStream()的参数可以 是文件路径也可以是File对象
  + 如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的
  + 如果文件已经存在，则会清空文件
  + write方法的参数是整数，实际上写到本地文件中的是整数在ASCII表上对于的字符

##### 书写格式：

  | 方法名称                             | 说明                         |
  | ------------------------------------ | ---------------------------- |
  | void write(int b)                    | 一次写一个字节数据           |
  | void write(byte[] b)                 | 一次写一个字节数组数据       |
  | void write(byte[] b,int off,int len) | 一次写一个字节数组的部分数据 |

  代码示例：

  ```java
  public class demo1 {
      public static void main(String[] args) throws IOException {
          FileOutputStream fos = new FileOutputStream("myio\\a.txt");
          byte[] bytes = {70,71,72};
          fos.write(bytes);
          fos.write(bytes,1,2);
          //参数一：数组
          //参数二：起始索引0
          //参数三：个数
          fos.close();
      }
  }
  ```

  ##### 换行写和续写

+ 写入一个换行符

  > windows：\r\n
  >
  > linux：\n
  >
  > Mac：\r

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("myio\\a.txt");
        String str = "www";
        byte[] byte1 = str.getBytes();
        fos.write(byte1);
        String huanhang = "\r\n";
        byte[] byte2 = huanhang.getBytes();
        fos.write(byte2);
        fos.write(70);
        fos.close();
    }
}
```

+ 续写

  `FileOutputStream(String str,true)`

  当第二个参数为true时开启续写，默认关闭续写

#### FileInputStream(字节输入流)

##### 书写步骤

> 1 创建字节输入流对象
>
> 2 读入数据
>
> 3 释放资源

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("myio\\a.txt");
        int b= fis.read();
        System.out.println(b);
        fis.close();
    }
}
```

+ read方法一次只读一个字节，读出来的是数据在ASCII上对于的数字
+ 如果read方法读不到数据，就会返回-1
+ new FileOutputStream()：如果文件不存在会直接报错

##### 循环读取

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("myio\\a.txt");
        int b;
        while ((b = fis.read()) != -1) {
            System.out.println(b);
        }
        fis.close();
    }
}
```

##### 文件拷贝

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        //1. 创建对象
        FileInputStream fis = new FileInputStream("myio\\movie.mp4");
        FileOutputStream fos = new FileOutputStream("myio\\copy.mp4");
        //2. 拷贝
        //核心思想：边读边写
        int b;
        while((b = fis.read())!=-1){
            fos.write(b);
        }
        //3. 释放资源
        //规则：先开的最后关
        fos.close();
        fis.close();
    }
}
```

+ FileInputStream一次只读取一个字节

多字节读取：

| 方法名称                       | 说明                   |
| ------------------------------ | ---------------------- |
| public int read()              | 一次读一个字节数据     |
| public int read(byte[ buffer]) | 一次读一个字节数组数据 |

+ 一次读一个字节数组数据，每次读取会尽可能把数组装满
+ 数组长度一般为1024的整数倍
