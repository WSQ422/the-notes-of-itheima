#### 概述和构造方法

+ File对象就表示一个路径，可以是文件的路径、也可以时文件夹的路径
+ 这个路径可以是存在的，也可以是不存在的

| 方法名称                                 | 说明                                               |
| ---------------------------------------- | -------------------------------------------------- |
| public File(String pathname)             | 根据文件路径创建文件对象                           |
| public File(String parent，String child) | 根据父路径名字符串和子路径名字符串创建文件对象     |
| public File(File parent，String child)   | 根据父路径对应文件对象和子路径名字符串创建文件对象 |

代码示例：

```java
 String str = "C:\\users\\alienware\\Desktop\\a.txt";
        File file = new File(str);
        System.out.println(file);
```

```java
        String parent ="C:\\users\\alienware\\Desktop";
        String child= "a.txt";
        File file1 = new File(parent, child);
```

```java
String parent = "C:\\users\\alienware\\Desktop";
        String child = "a.txt";
        File file2 = new File(parent);
        new File(file2, child);
```

#### File常见成员方法

##### 判断、获取

| 方法名称                 | 说明                                   |
| ------------------------ | -------------------------------------- |
| boolean isDirectory()    | 判断此路径名表示的File是否为文件夹     |
| boolean isFile()         | 判断此路径名表示的File是否为文件       |
| boolean exists()         | 判断此路径名表示的File是否存在         |
| long length()            | 返回文件的大小（字节数量）             |
| String getAbsolutePath() | 返回文件的绝对路径                     |
| String getPath()         | 返回定义文件时使用的路径               |
| String getName()         | 返回文件的名称（带后缀）               |
| long lastModified()      | 返回文件的最后修改时间（时间：毫秒值） |

+ length只能获取文件的大小，无法获取文件夹的大小
+ 如果getName方法的形参是一个文件，返回值为文件名+后缀名，如果为文件夹，返回值仅为文件夹的名字

##### 创建、删除

| 方法名称                | 说明               |
| ----------------------- | ------------------ |
| boolean createNewFile() | 创建一个新的空文件 |
| boolean  mkdir()        | 创建单级文件夹     |
| boolean mkdirs()        | 创建多级文件夹     |
| boolean delete()        | 删除文件、空文件夹 |

+ createNewFile ：

  >+ 如果当前路径表示的文件是不存在的，则创建成功，方法返回true
  >
  >+ 如果当前路径表示的文件是存在的，则创建失败，方法返回false
  >
  >+ 如果父级路径是不存在的，那么方法会有异常IOException
  >
  >+ createNewFile方法创建的一定是文件，如果路径中不包含后缀名，则创建一个没有后缀的文件

+ mkdir ：

  >+ windows当中的路径是唯一的，如果当前路径（文件或文件夹）已经存在，则创建失败，返回false
  >+ mkdir只能创建单级文件夹，不能创建多级文件夹
  
+ mkdirs（使用最多）既可以创建单级文件夹，也可以创建多级文件夹

+ delete ：

  > + 删除的文件不会放到回收站中，而是永久删除
  > + delete只能删除空文件夹，不能删除有内容的文件夹

##### 获取并遍历

| 方法名称           | 说明                               |
| ------------------ | ---------------------------------- |
| File[] listFiles() | 获取当前路径下所有内容并放入数组中 |

```java
    File f = new File("D:\\aaa");
        File[] files = f.listFiles();
        for(File file : files) {
            System.out.println(file);
```

> + 当调用者FIle表示的路径不存在时，返回null
> + 当调用者FIle表示的路径时文件时，返回null
> + 当调用者FIle表示的路径是一个空文件夹时时，返回一个长度为0的数组
> + 当调用者FIle表示的路径是需要访问权限的文件夹时，返回null
> + 当调用者FIle表示的路径是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回
> + 隐藏的文件和文件夹也能获取

| 方法名称                                | 说明                                     |
| --------------------------------------- | ---------------------------------------- |
| static File[] listRoots()               | 列出可用的文件系统根                     |
| String[] list()                         | 获取当前该路径下所有内容(仅仅获取名字)   |
| String[] list(FilenameFilter filter)    | 利用文件名过滤器获取当前路径下所有内容   |
| File[] listFiles(FileFilter filter)     | 利用文件名过滤器获取当前路径下的所有内容 |
| File[] listFiles(FilenameFilter filter) | 利用文件名过滤器获取当前路径下的所有内容 |

> `System.out.println(Arrays.toString(arr)`
>
> 这段代码里面的Arrays.toString 方法必须要有吗？
>
> 是的，`Arrays.toString()` 方法是必须的。它是用来将数组转换为字符串并输出的方法。如果不使用它，直接输出数组名称只会得到数组的引用值，而不是数组的内容。因此，为了输出数组的内容，需要使用`Arrays.toString() `方法。

+ 综合示例：

  ```java
  public class test2 {
      public static void main(String[] args) {
          findMP4();
      }
          public static void findMP4() {
          File[] arr = File.listRoots();
          for (File f : arr) {
              findMP4(f);
          }
      }
          public static void findMP4(File src) {
          //把盘符下面的所有文件和文件夹放入数组中
          File[] files = src.listFiles();
          //进行非空判断，如果不是需要权限访问的文件夹就进行遍历
          if (files != null){
              //遍历数组，获取所有文件和文件夹
              for (File file : files) {
                  String name = file.getName();
                  //判断这个路径是否为文件
                  if (file.isFile() && name.endsWith(".mp4")) {
                      //如果是就返回file的路径
                      System.out.println(file);
                      //如果是文件夹就进行递归
                  } else {
                      findMP4(file);
                  }
              }
          }
      }
  }
  ```

  
