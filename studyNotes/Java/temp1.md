+ **Java中的String类的对象不能像C++一样通过下标访问字符**
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
  
+ **Java中的变量重复定义的疑问**
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
