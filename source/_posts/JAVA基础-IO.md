---
title: JAVA基础-IO
tags: JAVA基础
toc: true
---

# 含义
IO，输入/输出，是任何编程语言都需要解决的问题。需要考虑的方面有多种：字符读取、字节读取；文件读取、网络读取、内存读取等等。
计算机系统中，IO操作往往是最耗时，并且限制了系统的运行速度。   
输入流可以理解为向内存输入，输出流可以理解为从内存输出。
# JAVA IO
在Java IO中，流是一个核心的概念。流从概念上来说是一个连续的数据流。
你既可以从流中读取数据，也可以往流中写数据。流与数据源或者数据流向的媒介相关联。
在Java IO中流既可以是字节流(以字节为单位进行读写)，也可以是字符流(以字符为单位进行读写)。
- 流式部分――IO的主体部分
- 非流式部分――主要包含一些辅助流式部分的类，如：File类、RandomAccessFile类和FileDescriptor等类
- 文件读取部分的与安全相关的类，如：SerializablePermission类。以及与本地操作系统相关的文件系统的类，如：FileSystem类和Win32FileSystem类和WinNTFileSystem类
JAVA中，inputStream或reader类用于从数据源读取数据，outputStream或writer用于向目标媒介写入数据。
流就是一个用来连接输入/输出与当前运行内存的对象，可以通过流从对应的输入（网络、文件、缓存）读取到运行内存，也可以向输出写数据。
# 特点
流的特点：One dimension，one direction。一维（流只有一个维度），单向（流是单向的，只能一个方向流动）。
# 用途
- 文件访问
- 网络访问
- 内存缓存访问
- 线程内部通信
- 缓冲
- 过滤
- 解析
- 读写文本
- 读写基本类型
- 读写对象
# IO流

||Byte Based|Byte Based|Character Based|Character Based|
|:----:|:----:|:----:|:----:|:----:|   
||input|output|input|output|
|Basic|InputStream|OutputStream|Reader<br>InputStreamReader|Writer<br>OutputStreamWriter|
|Arrays|ByteArrayInputStream|ByteArrayOutputStream|CharArrayReader|CharArrayWriter|
|Files|FileInputStream<br>RandomAccessFile|FileOutputStream<br>RandomAccessFile|FileReader|FileWriter|
|Pipes|PipedInputStream|PipedOutputStream|PipedReader|PipedWriter|
|Buffering|BufferedInputStream|BufferedOutputStream|BufferedReader|BufferedWriter|
|Filtering|FilterInputStream|FilterOutputStream|FilterReader|FilterWriter|
|Parsing|PushBackInputStream<br>StreamTokenizer||PushBackReader<br>LineNumberReader||
|Strings|||StringReader|StringWriter|
|Data|DataInputStream|DataOutputStream|||
|Data-Formatted||PrintStream||PrintWriter|
|Objects|ObjectInputStream||ObjectOutpurStream||
|Utilities|SequenceInputStream||||


其中的四大基本类别为`InputStream`, `OutputStream` , `Reader` 和`Writer`。

## 字节流
字节流通常以`stream`命名。除了DataInputStream 和DataOutputStream 还能够读写int, long, float和double类型的值以外，
其他流在一个操作时间内只能读取或者写入一个原始字节。

### InputStream
所有数据输入流都继承自InputStream。
通常使用输入流中的read()方法读取数据。read()方法返回一个整数，代表了读取到的字节的内容(0 ~ 255)。
当达到流末尾没有更多数据可以读取的时候，read()方法返回-1。
在InputStream中，有多个方法，需要子类进行实现。
- read()
- read(byte b[])
- int read(byte b[], int off, int len)
- skip(long n)
- available()
- close()
- mark(int readlimit)
- reset()
- markSupported()

其中，`mark(int readlimit)`方法，标记记录当前读取的位置；当调用`reset()`方法时，当前位置就会恢复到`mark`方法中记录的值，从而可以重复读取这些数据。`readlimit`表示可以记录的最长字节数。   





InputStream中的read方法是抽象方法，由各个子类实现。（FileInputStream会调用`private native int read0()`）。其操作对象是不同的数据源：文件、管道、序列化的流、字节数组、String对象


### FilterInputStream
FilterInputStream是实现自定义过滤输入流的基类，基本上它仅仅只是覆盖了InputStream中的所有方法。
除了构造函数取一个InputStream变量作为参数之外，我没看到FilterInputStream任何对InputStream新增或者修改的地方。
如果你选择继承FilterInputStream实现自定义的类，同样也可以直接继承自InputStream从而避免额外的类层级结构。

### BufferedInputStream
`BufferedInputStream`，可以一次读取最大值为8192字节的数据，到其内部的`byte buf[]`。在`BufferedInputStream`里，`mark`方法记录了当前位置`pos`（初始时`markpos = -1`）。一旦调用了
`mark`方法，则`markpos = pos;`。当调用`reset`方法，会执行`pos = markpos;`。
```
    public synchronized void mark(int readlimit) {
        marklimit = readlimit;
        markpos = pos;
    }
```
`BufferedInputStream`中,读取方法如下：
```
    public synchronized int read() throws IOException {
        if (pos >= count) {
            fill();
            if (pos >= count)
                return -1;
        }
        return getBufIfOpen()[pos++] & 0xff;
    }
```

```
getBufIfOpen()[pos++] & 0xff;
```
返回的是整型数据，因此，最后通过`&`操作将返回值转成整型。
注意：位运算是以二进制位为单位进行的运算，其操作数和运算结果都是整型值。

而其中的`fill()`方法
```
private void fill() throws IOException {
        byte[] buffer = getBufIfOpen();
        if (markpos < 0)
            pos = 0;            /* no mark: throw away the buffer 没有标记mark时，只需要pos置为0，重新读取数据*/
        else if (pos >= buffer.length)  /* no room left in buffer pos最大值应该是buffer.length，大于号何时成立还不知*/
            if (markpos > 0) {  /* can throw away early part of the buffer markops大于0，将markops到结尾的数据复制到0-sz的位置，然后sz后的数据再重新读取*/
                int sz = pos - markpos;
                System.arraycopy(buffer, markpos, buffer, 0, sz);
                pos = sz;
                markpos = 0;
            } else if (buffer.length >= marklimit) { /* markpos=0,marklimit<=buffer.length，由于此时pos已经大于count，markpos = 0，markpos与pos之间的距离已经大于buffer.length，而标记的marklimit又小于它，所以废除mark */
                markpos = -1;   /* buffer got too big, invalidate mark */
                pos = 0;        /* drop buffer contents */
            } else if (buffer.length >= MAX_BUFFER_SIZE) {/*数组过长*/
                throw new OutOfMemoryError("Required array size too large");
            } else {            /* grow buffer */* pos=buffer.length，markops=0,buffer.length正常，marklimit还允许继续有效，则扩容到marklimit */
                int nsz = (pos <= MAX_BUFFER_SIZE - pos) ?
                        pos * 2 : MAX_BUFFER_SIZE;
                if (nsz > marklimit)
                    nsz = marklimit;
                byte nbuf[] = new byte[nsz];
                System.arraycopy(buffer, 0, nbuf, 0, pos);
                if (!bufUpdater.compareAndSet(this, buffer, nbuf)) {
                    // Can't replace buf if there was an async close.
                    // Note: This would need to be changed if fill()
                    // is ever made accessible to multiple threads.
                    // But for now, the only way CAS can fail is via close.
                    // assert buf == null;
                    throw new IOException("Stream closed");
                }
                buffer = nbuf;
            }
        count = pos;
        int n = getInIfOpen().read(buffer, pos, buffer.length - pos);
        if (n > 0)
            count = n + pos;
    }
```

```
    public static void testReturnInt() throws IOException {
        File file = new File("E:\\1.txt");
        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(file));

        System.out.println(bufferedInputStream.read());
    }
```
在测试中，BufferedInputStream中的传入的InputStream是FielInputStream。
它的构造函数中必须传入InputStream对象，而FileInputStream可以用File创建，其他资源型的Stream都可以用对应的资源创建，才可以将创建好的InputSteam对象赋值给BufferedInputStream。  
个人理解，只有资源型的Stream真正实现了read方法（FileInputStream里是native方法），其他类型的Stream都是调用资源型的inputStream的read。

### OutputStream
OutputStream是所有输出流的基类。与InputStream类似，OutputSteam也定义了子类需要实现的方法。
- write(int b)
- write(byte b[])
- write(byte b[], int off, int len)
- flush()
- close()

#### BufferedOutputStream
实际上也是调用了自己内部的OutputStream的方法实现输出数据。


