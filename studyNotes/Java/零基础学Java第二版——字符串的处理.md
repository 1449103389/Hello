# 第六章 字符串的处理

## 6.1 字符串的基本概念

## 6.2 字符串处理的类库种类

### 6.2.2 String类
**String类有9种构造器，下面说六种：**
1. 默认构造器：String()
2. 字节参数的构造器：String(byte[] b)
```Java
public class ByteConstuctor{
    public static void main(String[] args){
        byte[] b = {97,98,99};
        String str = new String(b);
        System.out.println(str);
    }
}

output:
abc
```
3. 获取指定字节数的构造器：String(byte[] b, int offset, int length)，offset指定开始位置（从0开始），length指定长度
4. 将字节型数据转换为字符集输出的构造器：String(byte[] b, int offset, int length, String charsetName)，字符集一般
有"usascii"、"iso-8859-I"、"utf-8"、"utf-16"等
5. 字符数组的构造器：String(char[] c)
```Java
public class CharConstructor{
    public static void main(String[] args){
        char[] c = {'w','e','l','c','o','m','e'};
        String str = new String(c);
        System.out.println(str);
    }
}

output:
welcome
```
6. 截取部分字符串数组内容的构造器：String(char[] c, int offset, int count)
  
### 6.2.3 字符串处理的方法
1. 串连接：在Java语言中，有两种串连接的方法：一种是使用“+”，另一种是使用方法函数concat(String)
2. 提取子字符串：
   + substring(int index)：提取从index指定的位置开始，一直到字符串的最后
   + substring(int beginIndex, int endIndex)：提取从beginIndex开始，到endIndex结束的子串（前闭后开）
3. 从字符串中分解字符：charAt(int index)，返回index位置处的字符
4. **得到字符串的长度：length()方法，注意与数组长度的区别，数组长度是length，是一个属性值，字符串长度是方法**
5. 测试字符串是否相等：
   + equals(String str)：区别大小写
   + equalsIgnoreCase(String str): 忽略大小写
6. 查找特定子串：
   + indexOf(子串内容)：返回子串的位置，返回负数表示没查到
   + startsWith(子串内容)：测试当前字符串是否以一个子串开始
   + endsWith(子串内容)：测试当前字符串是否以子串内容为结尾
  
### 6.2.4 缓冲字符串处理类StringBuffer  
当创建StringBuffer类对象时，系统为对象分配的内容会自动扩展
  
### 6.2.5 缓冲字符串StringBuffer类的构造器
1. 默认的构造器：StringBuffer sb = new StringBuffer()，默认构造器是由系统自动分配容量，系统容量默认值是16个字符
```Java
//输出字符串的容量capacity
//输出字符串的长度length
public class TestStrBuf{
    public static void main(String[] args){
        StringBuffer sb = new StringBuffer();
        System.out.println(sb.capacity());
        System.out.println(sb.length());
    }
}

output:
16
0
```
2. 设定容量大小的构造器：StringBuffer sb = new StringBuffer(int x)
  
### 6.2.6 缓冲字符串处理的方法
