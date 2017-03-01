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
所有数据流都继承自InputStream（OutputSteam也是）。
通常使用输入流中的read()方法读取数据。read()方法返回一个整数，代表了读取到的字节的内容(0 ~ 255)。
当达到流末尾没有更多数据可以读取的时候，read()方法返回-1。
InputStream中的read方法是抽象方法，由各个子类实现。（FileInputStream中是native方法）。