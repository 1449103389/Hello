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
