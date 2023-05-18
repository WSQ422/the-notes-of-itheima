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

![](E:\the-notes-of-itheima\Image\IO流.png)

![](E:\the-notes-of-itheima\Image\IO流2.png)

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
+ 配合String方法使用

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("myio\\a.txt");
        byte[] bytes = new byte[2];
        int len1 = fis.read(bytes);
        String str1 = new String(bytes, 0, len1);
        System.out.println(str1);
        int len2 = fis.read(bytes);
        String str2 = new String(bytes, 0, len2);
        System.out.println(str2);
        int len3 = fis.read(bytes);
        String str3 = new String(bytes, 0, len3);
        System.out.println(str3);
    }
}
```

改进后：

```Java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("myio\\movie.mp4");
        FileOutputStream fos = new FileOutputStream("myio\\copy.mp4");
        int len;
        byte[] bytes = new byte[1024 * 1024 * 5];
        while ((len = fis.read(bytes)) != -1){
            fos.write(bytes,0,len);
        }
        fos.close();
        fis.close();
    }
}
```

##### IO流中捕获异常

+ 格式：

  ```java
  try{
      
  }catch{
      
  }finally{
      fos.close;
  }
  ```

  + finally中的代码是一定会执行的，除非JVM推出
  + 接口：AutoCloseable特定情况下，可以自动释放资源

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream("myio\\movie.mp4");
            fos = new FileOutputStream("myio\\copy.mp4");
            int len;
            byte[] bytes = new byte[1024 * 1024 * 5];
            while ((len = fis.read(bytes)) != -1) {
                fos.write(bytes, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
}
```

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("myio\\movie.mp4");
        FileOutputStream fos = new FileOutputStream("myio\\copy.mp4");
        try (fis;fos){
            int len;
            byte[] bytes = new byte[1024 * 1024 * 5];
            while ((len = fis.read(bytes)) != -1) {
                fos.write(bytes, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### 字符集

##### GBK

+ 一个英文使用一个字节存储

  > 二进制第一位是0

+ 一个汉字使用两个字节存储

  > 前面那个字节叫高位字节
  >
  > 后面那个字节叫低位字节
  >
  > 高位字节二进制一定以1开头，转成十进制之后是一个负数

##### Unicode

ASCII：1个字节

简体中文： 3个字节

+ UTF-8的编码格式：

  > 一个英文占一个字节，二进制第一位是0，转成十进制是正数
  >
  > 一个中文占三个字节，二进制第一位是1，第一个字节转成十进制是负数

#### Java中相关方法

##### 编码的方法

| String类中的方法                           | 说明                 |
| ------------------------------------------ | -------------------- |
| public byte[] getBytes()                   | 使用默认方式进行编码 |
| public byte[] getBytes(String charsetName) | 使用指定方式进行编码 |

##### 解码的方法

| String类中的方法                        | 说明                 |
| --------------------------------------- | -------------------- |
| String(byte[] bytes)                    | 使用默认方式进行解码 |
| String(byte[] bytes,String charsetName) | 使用指定方式进行解码 |

### 字符流

![](E:\the-notes-of-itheima\Image\字符流.png)

字符流的底层就是字节流

字符流=字节流+字符集

+ 特点：

  > 输入流：一次读入一个字节，遇到中文时，一次读多个字节
  >
  > 输出流：底层会把数据按照指定的编码方式进行编码，变成字节再写到文件中

#### FileReader

##### 书写步骤

> 1 创建字符输入流对象
>
> | 构造方法                           | 说明                       |
> | ---------------------------------- | -------------------------- |
> | public FileReader(File file)       | 创建字符输入流关联本地文件 |
> | public FileReader(String pathname) | 创建字符输入流关联本地文件 |
>
> + 如果文件不存在，直接报错
>
> 2 读取数据
>
> | 成员方法                       | 说明                         |
> | ------------------------------ | ---------------------------- |
> | public int read()              | 读取数据，读到末尾返回-1     |
> | public int read(char[] buffer) | 读取多个数据，读到末尾返回-1 |
>
> + 一次读入一个字节，遇到中文时，一次读多个字节
> + 空参read方法：底层还会进行解码并转成十进制
> + 有参read方法：底层还会解码，强转，把强转之后的字符放到数组中
>
> 3 释放资源
>
> | 成员方法           | 说明          |
> | ------------------ | ------------- |
> | public int close() | 释放资源/关流 |
>
> 

代码示例：

+ read空参：

```Java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("myio\\a.txt");
        int ch;
        while ((ch = fr.read()) != -1) {
            System.out.println((char) ch);
        }
        fr.close();
    }
}
```

+ read有参：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("myio\\a.txt");
        int ch;
        while ((ch = fr.read()) != -1) {
            System.out.println((char) ch);
        }
        fr.close();
    }
}
```

#### FileWriter

| 构造方法                                          | 说明                             |
| ------------------------------------------------- | -------------------------------- |
| public FileWriter(File file)                      | 创建字符输出流关联本地文件       |
| public FileWriter(String pathname)                | 创建字符输出流关联本地文件       |
| public FileWriter(File file,boolean append)       | 创建字符输出流关联本地文件，续写 |
| public FileWriter(String pathname,boolean append) | 创建字符输出流关联本地文件，续写 |

| 成员方法                                | 说明                   |
| --------------------------------------- | ---------------------- |
| void write(int c)                       | 写出一个字符           |
| void write(String str)                  | 写出一个字符串         |
| void write(String str,int off,int len)  | 写出一个字符串的一部分 |
| void write(char[] cbuf)                 | 写出一个字符数组       |
| void write(char[] cubf,int off,int len) | 写出字符数组的一部分   |

+ 步骤

  > 1 创建字符输出流对象
  >
  > + 如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的
  > + 如果文件已经存在，则会清空文件，如果不想清空可以打开续写开关
  >
  > 2 写数据
  >
  > 3 释放资源
  >
  > | 成员方法            | 说明                             |
  > | ------------------- | -------------------------------- |
  > | public int close()  | 释放资源/关流                    |
  > | public void flush() | 将缓冲区中的数据刷新到本地文件中 |
  >
  > + flush刷新之后还可以继续往文件中写出数据

代码示例：

+ 拷贝文件夹：

  ```Java
  public class demo1 {
      public static void main(String[] args) throws IOException {
          File src = new File("myio\\aaa\\src");
          File dest = new File("myio\\aaa\\dest");
          copydir(src, dest);
      }
      private static void copydir(File src, File dest) throws IOException {
          dest.mkdir();
          //进入数据源
          File[] files = src.listFiles();
          //遍历文件
          for (File file : files) {
              //是文件就拷贝
              if (file.isFile()) {
                  FileInputStream fis = new FileInputStream(src);
                  FileOutputStream fos = new FileOutputStream(new File(dest,file.getName()));
                  byte[] bytes = new byte[1024];
                  int len;
                  while ((len = fis.read(bytes)) != -1) {
                      fos.write(bytes, 0, len);
                  }
                  //是文件夹就递归
              } else {
                  copydir(file,new File(dest,file.getName()));
  
              }
          }
      }
  }
  ```

+ 加密文件:

  利用异或同一个数两次得到原来的数值的原理

### 字节缓冲流

+ 底层自带了长度为8192的缓冲区来提高性能

| 方法名称                                     | 说明                                     |
| -------------------------------------------- | ---------------------------------------- |
| public BufferedInputStream(InputStream is)   | 把基本流包装成高级流，提高读取数据的性能 |
| public BufferedOutputStream(OutputStream os) | 把基本流包装成高级流，提高读取数据的性能 |

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("myio\\a.txt"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myio\\b.txt"));
        int len;
        while((len= bis.read())!=-1){
            bos.write(len);
        }
        bos.close();
        bis.close();
    }
```

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("myio\\a.txt"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myio\\b.txt"));
        int len;
        byte[] bytes = new byte[1024];
        while((len= bis.read(bytes))!=-1){
            bos.write(bytes,0,len);
        }
        bos.close();
        bis.close();
    }      
}
```

### 字符缓冲流

+ 底层自带了长度为8192的缓冲区来提高性能

| 方法名称                        | 说明               |
| ------------------------------- | ------------------ |
| public BufferedReader(Reader r) | 把基本流变成高级流 |
| public BufferedWriter(Writer r) | 把基本流变成高级流 |

+ BufferedWriter方法：如果没有该文件就会自动创建一个新文件，如果有就会清空文件，如果不想清空文件可以打开续写（在基本流FileWriter的括号中打开续写）
+ 特有方法：

| 字符缓冲输入流特有方法   | 说明                                     |
| ------------------------ | ---------------------------------------- |
| public String readline() | 读取一行数据，如果没有数据可读，返回null |

+ readline方法在读取的时候一次读一整行，遇到回车换行就停止，但是不会把回车换行也读取到内存中

| 字符缓冲输出流特有方法 | 说明         |
| ---------------------- | ------------ |
| public void newLine()  | 跨平台的换行 |

### 转换流

+ 字符流和字节流之间的桥梁

![](E:\the-notes-of-itheima\Image\IO流3.png)

+ 作用：

  > 1 指定字符集读写（已淘汰）
  >
  > 2 字节流想要使用字符流中的方法

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        //1.已淘汰
       InputStreamReader isr = new InputStreamReader(new FileInputStream("myio\\a.txt"), "GBK");
        int ch;
        while ((ch = isr.read()) != -1) {
            System.out.println((char) ch);
        }
        isr.close();
        
        //2.
        FileReader fr = new FileReader("myio\\a.txt", Charset.forName("GBK"));
        int ch;
        while ((ch = fr.read()) != -1) {
            System.out.println((char) ch);
        }
        fr.close();
    }
}
```

### 序列化流/对象操作输出流

把java中的对象写到本地文件中

![](E:\the-notes-of-itheima\Image\IO流4.png)

| 构造方法                                    | 说明                 |
| ------------------------------------------- | -------------------- |
| public ObjectOutputStream(OutputStream out) | 把基本流包装成高级流 |

| 成员方法                                  | 说明                       |
| ----------------------------------------- | -------------------------- |
| public final void writeObject(object obj) | 把对象序列化写出到文件中去 |

> 使用对象输出流将对象保存到文件时出现NotSerializableException异常
>
> 解决方法：让javabean类实现Serializable接口

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        Student stu = new Student("zhangsan", 23);
        ObjectOutputStream ojs = new ObjectOutputStream(new FileOutputStream("myio\\a.txt"));
        ojs.writeObject(stu);
        ojs.close();
    }
}
```

### 反序列化流/对象操作输入流

把序列化到本地中的对象读取到程序中

| 构造方法                                  | 说明               |
| ----------------------------------------- | ------------------ |
| public ObjectInputStream(InputStream out) | 把基本流变成高级流 |

| 成员方法                   | 说明                                 |
| -------------------------- | ------------------------------------ |
| public Object readObject() | 把序列化到本地中的对象读取到程序中来 |

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("myio\\a.txt"));
        Student o = (Student) ois.readObject();
        System.out.println(o);
        ois.close();
    }
}
```

+  javabean类中加入序列号

  ```java
  public class Student {
      private static final long serialVersionUID = 1L;
      private String name;
      private int age;
  }
  ```
  如果类中有一个变量不想保存到本地文件，可以使用瞬态关键字transient去修饰这个变量
  
  ```java
  private transient int age;
  ```
  

 ### 打印流

打印流一般分为：PrintStream,PrintWriter两个类

+ 打印流只操作文件目的地，不操作数据源

+ 特有的写出方法可以实现数据原样写出

  > 例如：打印：97   文件中：97

+ 特有的写出方法可以实现自动刷新，自动换行

  > 打印一次数据=写出+换行+刷新

![](E:\the-notes-of-itheima\Image\IO流5.png)

#### 字节打印流

| 构造方法                                                     | 说明                         |
| ------------------------------------------------------------ | ---------------------------- |
| public PrintStream(OutputStream/File/String)                 | 关联字节输出流/文件/文件路径 |
| public PrintStream(String filename,Charset charset)          | 指定字符编码                 |
| public PrintStream(OutputStream out,boolean autoFlush)       | 自动刷新                     |
| public PrintStream(OutputStream out,boolean autoFlush,String encoding) | 指定字符编码且自动刷新       |

+ 字节流底层没有缓冲区，开不开自动刷新都一样

| 成员方法                                        | 说明                                       |
| ----------------------------------------------- | ------------------------------------------ |
| public void write(int b)                        | 常规方法：规则和之前一样，将指定的字节写出 |
| public void println(XXX xx)                     | 特有方法：打印任意数据，自动刷新，自动换行 |
| public void print(XXX xx)                       | 特有方法：打印任意数据，不换行             |
| public void printf(String format,Object...args) | 特有方法：带有占位符的打印语句，不换行     |

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws FileNotFoundException {
        PrintStream ps = new PrintStream(new FileOutputStream("myio\\a.txt"));
        ps.println(97);
        ps.print(true);
        ps.printf("%s爱上了%s", "阿珍", "阿强");
        ps.close();
    }
}
```

#### 字符打印流

+ 底层有缓冲区，想要自动刷新需要手动开启

| 构造方法                                                     | 说明                         |
| ------------------------------------------------------------ | ---------------------------- |
| public PrintWriter(Write/File/String)                        | 关联字节输出流/文件/文件路径 |
| public PrintWriter(String filename,Charset charset)          | 指定字符编码                 |
| public PrintWriter(Write w,boolean autoFlush)                | 自动刷新                     |
| public PrintWriter(OutputStream out,boolean autoFlush,Charset charset) | 指定字符编码且自动刷新       |

| 成员方法                                        | 说明                                       |
| ----------------------------------------------- | ------------------------------------------ |
| public void write(...)                          | 常规方法：规则和之前一样，将指定的字节写出 |
| public void println(XXX xx)                     | 特有方法：打印任意数据，自动刷新，自动换行 |
| public void print(XXX xx)                       | 特有方法：打印任意数据，不换行             |
| public void printf(String format,Object...args) | 特有方法：带有占位符的打印语句，不换行     |

### 解压缩流/压缩流

![](E:\the-notes-of-itheima\Image\IO流6.png)

#### 解压缩流ZipInputStream

本质：把每一个ZipEntry对象按照层级拷贝到本地的另一个文件夹中

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        File src = new File("myio\\a");
        File dest = new File("myio\\");
        unzip(src, dest);
    }
    public static void unzip(File src, File dest) throws IOException {
        //创建一个解压缩流来读取压缩包中的数据
        ZipInputStream zip = new ZipInputStream(new FileInputStream(src));
        ZipEntry entry;//创建一个变量来表示压缩包中的文件和文件夹
        while ((entry = zip.getNextEntry()) != null) {
            if (entry.isDirectory()) {
                File file = new File(dest, entry.toString());
                file.mkdir();
            } else {
                FileOutputStream fos = new FileOutputStream(new File(dest, entry.toString()));
                int len;
                while ((len = zip.read()) != -1) {
                    fos.write(len);
                }
                fos.close();
                zip.closeEntry();
            }
        }
        zip.close();
    }
}
```

#### 压缩流ZipOutputStream

本质：把每一个（文件/文件夹）看成ZipEntry对象放到压缩包中

代码示例：

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        File src = new File("myio\\a");
        File dest = new File("myio\\");
        tozip(src, dest);
    }
    public static void tozip(File src, File dest) throws IOException {
        //1.创建压缩流关联压缩包
        ZipOutputStream zop = new ZipOutputStream(new FileOutputStream(src,"a.zip"));
        //2.创建ZipEntry对象放到压缩包当中
        ZipEntry entry = new ZipEntry("a.txt");
        //3.把ZipEntry对象放到压缩包中
        zop.putNextEntry(entry);
        //4.把src文件中的数据写到压缩包文件中
        FileInputStream fis = new FileInputStream(src);
        int d;
        while ((d = fis.read()) != -1) {
            zop.write(d);
        }
        zop.closeEntry();
        zop.close();
        fis.close();
    }
}
```

