+ **1. Java中的String类的对象不能像C++一样通过下标访问字符**
```Java
public class Temp{
	public static void main(String[] args){
		String str = "abc";
		str[0] = 'x';
		System.out.println(str);
	}
}

编译时报错：需要数组，但找到String
          str[0] = 'x';
```       
  
+ **2. Java中的变量重复定义的疑问**
```Java
//1.
public class Temp{
	public static void main(String[] args){
		int i = 0;
		for(int j=0; j<1; j++){
			int i = j;
			System.out.println(i);
		}
	}
}

编译时报错：已在方法mian(String[] args)中定义了变量i
		int i = j;
		
//2.
public class Temp{
	static int i = 1;
	public static void main(String[] args){
		for(int j=0; j<1; j++){
			int i = j;
			System.out.println(i);
		}
	}
}

编译时不报错，output:
0

//3.
public class Temp{
	static int i = 1;
	public static void main(String[] args){
		for(int j=0; j<1; j++){
			System.out.println(i);
		}
	}
}

编译时不报错，output:
1

为啥类变量在方法中就能重复定义，并且还会把类变量覆盖？
```

+ **3. 在继承中，子类对父类构造器的调用必须在最前面**
```Java
public class Temp{
	public Temp(){
		System.out.println("parent constructor");
	}
	public static void main(String[] args){
		new TempSon();
	}
}

class TempSon extends Temp{
	public TempSon(){
		System.out.println("son constructor");
		super();
	}
}

编译时报错： 对super()的调用必须是构造器中的第一个语句
		super();
		
如果编写子类构造器时不显式调用父类构造器，那么默认由编译器隐式调用super()
```
  
+ **4. Java中子类和父类间的转换**
```Java
public class Temp{
	public void printInfo(){
		System.out.println("parent info");
	}
	
	public static void main(String[] args){
        	Temp t1 = new TempSon();
        	t1.printInfo();
		//!t1.sonFunction();//编译报错，找不到符号
		t1 = (TempSon)t1;
		t1.printInfo();
		//!t1.sonFunction();//编译报错，找不到符号
		Temp t2 = new Temp();
		t2.printInfo();
		t2 = (TempSon)t2;//编译时不报错，但运行时会抛出异常(源码中这是第14行)
    }
    
}

class TempSon extends Temp{
	public void printInfo(){
		System.out.println("son info");
	}
	
	public void sonFunction(){
		System.out.println("only in son class");
	}
}

output:
son info
son info
parent info
Exception in thread "main" java.lang.ClassCastException: Temp cannot be cast to TempSon
        at Temp.main(Temp.java:14)
	
    上例说明，初始化为子类的父类引用，在调用父类和子类都有的方法时，会根据动态绑定，调用子类的方法，但是，这个引用
不能调用只存在于子类中的方法（更准确地说是成员），即使在用类型转换转为子类后也不行。而初始化为父类的引用，在用类型
转换转为子类后，会在运行时抛出异常。
```
  
+ **5. String和StringBuffer**
```Java
public class Temp{
    public static void main(String[] args){
        String str1 = "abc";
		String str2 = str1;
		str2 = "def";
		StringBuffer sb1 = new StringBuffer("abc");
		StringBuffer sb2 = sb1;
		sb2.setCharAt(0,'x');
		System.out.println("str1:" + str1 +" str2:" + str2);
		System.out.println(" sb1:" + sb1 +"  sb2:" + sb2);
    }
}

output:
str1:abc str2:def
 sb1:xbc  sb2:xbc
```
