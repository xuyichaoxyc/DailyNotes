[TOC]



### 一、Java IO概览

IO的分类，数据格式（基于字节、基于字符）和传输方式（磁盘IO、网络IO），即将什么样的数据传输到哪里去。

操纵数据的方式（字节、字符）可以组合，转换，指定流最终写到什么地方

1. File（磁盘IO）
2. OutputStream、InputStream（基于字节传输）
3. Writer、Reader（基于字符传输）
4. Serializable（序列化）
5. Socket（网络IO）
6. NIO

### 二、磁盘操作（File）

File 类可以用于表示文件和目录的信息，但是它不表示文件的内容。

#### 磁盘 I/O 工作机制

读取和写入磁盘文件，系统调用（write()、read()，访问物理设备都需系统调用）

用户空间

内核空间

磁盘空间

如何访问磁盘文件？

1. 标准访问文件的方式

应用程序调用 read() 接口，os检查内核的高速缓存，有缓存则从缓存返回，无缓存则从磁盘中读取。

应用程序调用 write() 接口，数据从用户空间复制到内核空间，对于用户程序来说写操作已经完成，什么时候从内核地址空间写入到磁盘中由操作系统决定，除非显式调用了 sync 同步命令。

2. 直接 I/O 方式

应用程序直接访问磁盘数据，不经过内核空间，减少一次内核到用户空间的数据复制。

应用场景：对数据的缓存管理由应用程序实现的数据库管理系统中

3. 同步访问文件的方式

数据的读取和写入都是同步操作的，只有当数据被成功写入到磁盘时才返回给应用程序成功的标志。

场景：安全性要求较高

4. 异步方式

线程发出请求，处理其他，数据返回继续处理。

5. 内存映射方式

操作系统将内存中的某一块区域与磁盘中的文件关联起来

#### 递归列出一个目录下所有文件

```java
public static void listAllFiles(File dir){
    if(dir == null || !dir.exists()){
		return;
	}
    if(dir.isFile()){
		System.out.println(dir.getName());
		// System.out.println(dir.get);
		return;
    }
    for(File file : dir.listFiles()){
		listAllFiles(file);
	}
}
public static void main(String[] args){
    File file = new File("E:\\xyc_java");
    listAllFiles(file);
}
```

#### 磁盘文件的读写

```java
public static void writeFile(String dir, String content, boolean flag) throws IOException {
        try{
            // 字节输出流（文件字节输出流）
            OutputStream fos = new FileOutputStream(dir, flag);
            // 字符输出流
            OutputStreamWriter osw = new OutputStreamWriter(fos);

            osw.write(content, 0, content.length());
            osw.flush();
            osw.close();
        }
        catch (FileNotFoundException e){
            File write = new File(dir);
            BufferedWriter bw = new BufferedWriter(new FileWriter(write, flag));
            bw.write(content + "/r/n");
            bw.close();
        }
    }

    public static void readFile(String dir) throws IOException {
        StringBuffer buffer = null;

        buffer = new StringBuffer();
        try {
            // 字节输入流(文件字节输入流)
            InputStream fis = new FileInputStream(dir);
            // 字符输入流
            BufferedReader reader = new BufferedReader(new InputStreamReader(fis));

            String line = null;
            line = reader.readLine();
            while(line != null){
                buffer.append(line);
                buffer.append("/n");
                line = reader.readLine();
            }
            System.out.println(buffer);
            fis.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) throws IOException {
        String dirw = "./outtest.txt";

        String content = "jfeioanva\n2302492094\n";
        String result = null;

        writeFile(dirw, content, true);
        readFile(dirw);

    }
```

### 三、基于字节的操作

#### 基于字节的输入输出流

1. InputStream
   1. ByteArrayInputStream
   2. FileInputStream
      1. SocketInputStream
   3. FilterInputStream
      1. InflaterInputStream
         1. ZipInputStream
      2. BufferedInputStream
      3. DataInputStream
   4. ObjectInputStream
   5. PipedInputStream
2. OutputStream
   1. ByteArrayOutStream
   2. FileOutStream
      1. SocketOutStream
   3. FilterOutStream
      1. InflaterOutStream
         1. ZipOutStream
      2. BufferedOutStream
      3. DataOutStream
   4. ObjectOutStream
   5. PipedOutStream

#### 文件复制

```java
public static void copyFile(String src, String dist) throws IOException {
    FileInputStream in = new FileInputStream(src);
    FileOutputStream out = new FileOutputStream(dist);

    byte[] buffer = new byte[20 * 1024];
    int cnt;

    while((cnt = in.read(buffer, 0, buffer.length)) != -1){
        out.write(buffer, 0, cnt);
    }

    in.close();
    out.close();
}

public static void main(String[] args) {
    String src = "./outtest.txt";
    String dist = "./copytest.txt";

    try {
        copyFile(src, dist);
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

### 四、基于字符的操作

#### 基于字符的输入输出流

1. Writer
   1. OutputStreamWriter
      1. FileWriter
   2. BufferedWriter
   3. StringWriter
   4. PipedWriter
   5. PrintWriter
   6. CharArrayWriter
2. Reader
   1. InputStreamReader
      1. FileReader
   2. BufferedReader
   3. CharArrayReader
   4. FilterReader
   5. PipedReader
   6. StringReader

#### 字节与字符的转化

InputStreamReader 类是从字节到字符的转化桥梁，从InputStream到Reader的过程要指定编码字符集，否则将采用操作系统默认的字符集。

### 五、对象操作（序列化与反序列化，Serializable）



### 六、网络操作（Socket）



### 七、NIO



### 八、适配器模式



### 九、装饰者模式

