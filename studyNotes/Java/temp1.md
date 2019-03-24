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
