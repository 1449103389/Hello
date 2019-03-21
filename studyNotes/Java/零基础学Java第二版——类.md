# 第七章 类
  
## 7.1 JDK5和JDK6特性
### 7.1.1 什么是JDK
JDK(Java Development Kit)：Java开发工具包
### 7.1.2 JDK5的特点  
JDK1.5的一个重要主题就是通过新增一些特性来简化开发，包括泛型、for-each循环、自动装包/拆包、枚举、可变参数、静态导入
+ for-each循环：简化了集合的遍历
+ 自动装包/拆包：自动装包就是基本类型自动转为包装类，自动拆包就是包装类自动转为基本类型。在JDK1.5之前，总是对集合不能存放  
基本类型而耿耿于怀，自动转换机制解决了问题
+ 可变参数：可变参数的出现，使程序员可以声明一个接受可变参数数目的方法，（注 可变参数必须是函数声明中的最后一个参数）
+ 静态导入：要使用静态成员，必须给出提供这个方法的类。使用静态导入可以使被导入类的所有静态成员在当前类中直接可见，从而在使  
用这些静态成员时无须再给出它们的类名  
  
## 7.3 Java中已有的类
### 7.3.1 Java中的数学运算处理类Math  
Math类中的所有成员都是静态的，可以直接通过 **Math.成员** 访问
+ Math.round(x)：返回四舍五入的值
+ Math.floor(x)：返回不大于x的最大整数  
### 7.3.2 测试时间和日期的类Date  
Date类中的成员不是静态的，所以要用 **对象.成员** 访问
### 7.3.3 测试日历的类GregorianCalendar  
GregorianCalendar类是Calendar类的一个扩展，Calendar类是从总体上描述历法的类
  
### 7.4.2 设置器和访问器  
在Java语言中，把"set"函数称为设置器，把"get"函数称为访问器
```Java
public class TestSetter{
    private int a;
    public static void main(String[] args){
        TestSetter t = new TestSetter();
        t.setA(6);
        System.out.println(t);
    }
    
    public String toString(){
        return new String(Integer.toString(a));
    }
    
    public void setA(int a){
        a = a;
    }
}

发现一个问题，对于设置器，经常能看到赋值的时候都是用"this.变量=变量"的形式，我就试着不用this，直接用"变量=变量"的形式，发现
输出有点问题，比如上例，输出就是0而不是6，但是如果我把形参a改为x，输出就回变成我们想要的6，达到与使用this相同的效果.这说明，
设置器里使用this，其实就是为了让对象的成员和形参区别开来，达到正确赋值的目的
```
