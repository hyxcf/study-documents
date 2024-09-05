# 				IO流

![image-20240619224223934](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240619224223934.png)



##### 流的分类

- 按操作数据单位不同分为:字节流(8 bit) 二进制文件，字符流(按字符)文本文件
- 按数据流的流向不同分为:输入流，输出流
- 按流的角色的不同分为:节点流，处理流/包装流

### 字节流

- ##### FileInputStream:文件输入流

```java
@Test
    public void readFile02() throws IOException {
        String filePath = "e:\\hello.txt";
        // 字节数组
        byte[] buf = new byte[8];//一次读取8个字节
        int readLen = 0;
        FileInputStream fileInputStream = null;
        try {
            // 创建 FileInputStream 对象,用于读取文件
            fileInputStream = new FileInputStream(filePath);
            // 如果读取正常，返回实际读取的字节数
            while ((readLen = fileInputStream.read(buf)) != -1) {
                System.out.print(new String(buf,0, readLen));// 转成char显示
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 关闭文件流，释放资源
            fileInputStream.close();
        }
    }
```

- ##### FileOutputStream：文件输出流

```java
    @Test
    public void writeFile(){
        String filepath = "e:\\hello.txt";
        FileOutputStream fileOutputStream = null;
        try {

            // 1.new FileOutputStream(filepath):创建方式，当写入内容是，会覆盖原来的内容
            // 2.new FileOutputStream(filepath，true):创建方式,当写入内容是,是追加到文件后面
            fileOutputStream = new FileOutputStream(filepath,true);
            // 1.写入一个字节
//            fileOutputStream.write('a');
            // 2.写入字符串
            String str = "hyx,world";
            // str.getBytes():可以把字符串 -> 字节数组
            fileOutputStream.write(str.getBytes());
            // 3.write(byte[] b, int off, int len）将len字节从位于偏移量 off的指定字节数
            fileOutputStream.write(str.getBytes(),0,3);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }finally {
            try {
                fileOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
```



##### 处理流-BufferedInputStream和BufferedOutputStream

BufferedInputStream：

BufferedInputStream是字节流，在创建BufferedInputStream时，会创建一个内部缓冲区数组.

BufferedOutputStream：

BufferedOutputStream是字节流,实现缓冲的输出流，可以将多个字节写入底层输出流中，而不必对每次字节写入调用底层系统

```java
/**
 * hyx
 * 字节缓冲流
 * 演示使用 BufferedOutputStream利 BufferedInputStream 使用
 */
public class BufferedCopy2 {
    public static void main(String[] args) throws IOException {
        String srcPath = "e:\\周杰伦 - 暗号.mp3";
        String destPath = "f:\\暗号.mp3";
        BufferedInputStream bufferedInputStream = null;
        BufferedOutputStream bufferedOutputStream = null;
        try {
            bufferedInputStream = new BufferedInputStream(new FileInputStream(srcPath));
            bufferedOutputStream = new BufferedOutputStream(new FileOutputStream(destPath));
            // 定义数组进行读取
            byte[] buf = new byte[1024];
            int readLog = 0;
            while ((readLog = bufferedInputStream.read(buf)) != -1) {
                bufferedOutputStream.write(buf,0,readLog);
            }
            System.out.println("文件拷贝完毕");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }finally {
            bufferedOutputStream.close();
            bufferedInputStream.close();
        }
    }
}
```



##### 对象流-ObjectlnputStream和ObjectOutputStream（实现的接口必须有Serializable）

- ##### ObjectOutputStream

```java
public class ObjectOutPutStream_ {
    public static void main(String[] args) throws IOException {
        // 序列化后，保存的文件格式，不是存文本，而是按照他的格式来保存
        String filePath = "e:\\note.txt";
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));
        // 序列化数据到 e:\note.txt
        oos.writeInt(100);// int -> Integer(实现了Serializable)
        oos.writeBoolean(true); // boolean ->Boolean (实现了Serializable)
        oos.writeChar('a'); // char -> Charter (实现了Serializable)
        oos.writeDouble(9.9); // double -> Double (实现了Serializable)
        oos.writeUTF("黄豫湘"); // String (实现了Serializable)
        // 保存一个 Dog 对象
        oos.writeObject(new Dog("阿毕", 24));
        oos.close();
        System.out.println("数据保存完毕(序列化形式)");
    }
}
```

- ##### ObjectlnputStream

```java
public class ObjectInputStream_ {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        // 指定反序列化的文件
        String filePath = "e:\\note.txt";
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filePath));
        // 读取
        // 读取(反序列化)的顺序需要和你保存数据(序列化)的顺序一致,否则会出现异常
        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());
        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());
        Object o = ois.readObject();
        System.out.println(o.getClass());
        System.out.println(o);
        // 这里是特别重要的细节
        ois.close();
    }
}
```







### 字符流：

##### FileReader 和 FileWriter介绍

FileReader和 FileWriter是**字符**流，即按照字符来操作IO

- FileReader相关方法:

1. new FileReader(File/String)

2. read:每次读取单个字符，返回该字符，如果到文件末尾返回-1

3. read(char[]):批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回.

  相关APl:

    1) new String(char):将char[]转换成String
    2) new String(char[],off,len):将char[]的指定部分转换成String

```java
/**
     *  字符数组读取文件
     */
    @Test
    public void readFile02() {
        // 1.创建FileReader
        String filePath = "e:\\hello.txt";
        FileReader fileReader = null;
        int readLen = 0;
        char[] buf = new char[1024]; // 设置缓冲数组
        try {
            fileReader = new FileReader(filePath);
            while ((readLen = fileReader.read(buf)) != -1) {
                System.out.print(new String(buf,0,readLen));
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (fileReader != null) {
                    fileReader.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

        }
    }

    /**
     *  单个字符读取文件
     */
    @Test
    public void readFile01() {
        // 1.创建FileReader
        String filePath = "e:\\hello.txt";
        FileReader fileReader = null;
        int data = ' ';
        try {
            fileReader = new FileReader(filePath);
            while ((data = fileReader.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (fileReader != null) {
                    fileReader.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

        }
    }
}
```



- FileWriter常用方法

  1. new FileWriter(File/String):覆盖模式，相当于流的指针在首端2

  2. new FileWriter(File/String,true):追加模式，相当于流的指针在尾端

  3. write(int):写入单个字符

  4. write(char):写入指定数组

  5. write(char[],off,len):写入指定数组的指定部分

  6. write (string):写入整个字符串

  7. write(string,off,len):写入字符串的指定部分

    相关APl: String类:**toCharArray:将String转换成char[]**

    注意:
    **FileWriter使用后，必须要关闭(close)或刷新(flush)，否则写入不到指定的文件!**

```java
    @Test
    public void writeFile() {
        String filePath = "e:\\note.txt";
        FileWriter fileWriter = null;
        try {
            fileWriter = new FileWriter(filePath,true);
            // * 1) write(int):写入单个字符
            fileWriter.write('s');
            // * 2) write(char):写入指定数组
            char[] chars = {'a','b','c'};
            fileWriter.write(chars);
            // * 3) write(char[],off,len):写入指定数组的指定部分
            fileWriter.write("黄豫湘".toCharArray(),0,chars.length);
            // * 4) write (string):写入整个字符串
            fileWriter.write("你好 南阳");
            // * 5) write(string,off,len):写入字符串的指定部分
            fileWriter.write("你好 南阳",0,3);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        try {
            if (fileWriter != null) {
                fileWriter.close();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);

        }
    }
```



### 节点流和处理流

1. 节点流可以从一个特定的数据源*读写数据*,如FileReader、FileWriter

2. 处理流(也叫包装流)是“连接”在已存在的流（节点流或处理流)之上，为程序提供更为强大的读写功能,如

   BufferedReader，BufferedWriter

- 节点流和处理流一览图

![image-20240619140524138](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240619140524138.png)  

- 节点流和处理流的区别和联系
  1. 节点流是底层流/低级流,直接跟数据源相接。
  2. 处理流(包装流)包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出。[源码理解]
  3. 处理流(也叫*包装流*)对节点流进行包装，使用了*修饰器设计模式*，不会直接与数据源相连[模拟修饰器设计模式]
- 处理流的功能主要体现在以下两个方面:
  1. 性能的提高:主要以增加缓冲的方式来提高输入输出的效率。
  2. 操作的便捷:处理流可能提供了一系列便捷的方法来一次输入输出大批量的数据,使用更加灵活方便



- ##### 处理流-BufferedReader和BufferedWriter（需要.flush手动刷新，否则不会写入）

  BufferedReader 和 BufferedWriter属于字符流，是按照字符来读取数据的关闭时，只需要关闭外层流即可。

  1) BufferedReader和 BufferedWriter是安装字符操作
  2) 不要去操作二进制文件，可能造成文件损坏

BufferedReader ：

```java
 // 1.BufferedReader和 BufferedWriter是安装字符操作
 // 2.不要去操作二进制文件，可能造成文件损坏
public class BufferedReader_ {
    public static void main(String[] args) throws IOException {
        String filePath = "e:\\note.txt";
        // 创建 BufferedReader 对象
        BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
        // 读取
        String line;//按行读取
        // bufferedReader.readLine() 是按行读取文件
        // 2.当返回null时，表示文件读取完毕
        while ((line = bufferedReader.readLine()) != null) {
            System.out.println(line);
        }
        // 关闭流，只需要关闭外层流即可。因为底层会自动的去关闭节点流
        bufferedReader.close();

    }
}
```

 BufferedWriter：

```java
public class BufferedWriter_ {
    public static void main(String[] args) throws IOException {
        String filePath = "e:\\note.txt";
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath));

        bufferedWriter.write("hello，黄豫湘");
        bufferedWriter.newLine();// 插入一个和系统相关的换行符
        bufferedWriter.write("hello2，黄豫湘");
        bufferedWriter.newLine();// 插入一个和系统相关的换行符
        bufferedWriter.write("hello3，黄豫湘");

        // 关闭流
        bufferedWriter.close();
    }
}
```



#### Properties 类

读取 mysql.properties文件

```java
public class Properties02 {
    public static void main(String[] args) throws IOException {
        // 使用 Properties 类 来读取 mysql.properties文件

        // 1.创建对象
        Properties properties = new Properties();
        // 2.加载指定的配置文件
        properties.load(new FileReader("src\\mysql.properties"));
        // 3.把键值对显示到控制台
        properties.list(System.out);
        // 4.根据key 获取对应的值
        String user = properties.getProperty("user");
        String pwd = properties.getProperty("pwd");
        System.out.println("用户名=" + user);
        System.out.println("密码=" + pwd);

    }
}
```

修改 mysql.properties文件

```java
public class Properties03 {
    public static void main(String[] args) throws IOException {
        // 使用 Properties 类 来修改 mysql.properties文件

        // 创建对象
        // 1.如果该文件没有key就是创建
        // 2.如果该文件有key ,就是修改
        Properties properties = new Properties();
        // 创建文件
        properties.setProperty("charset", "utf-8");
        properties.setProperty("user", "scc");
        properties.setProperty("pwd", "123345");

        // 将k-v 存储文件中即可
        properties.store(new FileOutputStream("src\\mysql2.properties"), "hello world");
        System.out.println("保存文件成功");

    }
}
```











