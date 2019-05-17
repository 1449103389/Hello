1. **通过多态可以使指向子类对象的父类引用改变子类对象的独有属性**
```Java
public class Temp{
	private int t = 0;
	
	public void changeValue(){
		t = 3;
	}
	
	public String toString(){
		return new String("t: " + t);
	}
	
	public static void main(String[] args){
		Temp tp = new TempSon();
		tp.changeValue();
		System.out.println(tp);
	}
}

class TempSon extends Temp{
	private int ts = 0;
	
	public void changeValue(){
		super.changeValue();
		ts = 6;
	}
	
	public String toString(){
		return new String(super.toString() + " ts: " + ts);
	}
}

output:
t: 3 ts: 6
```

2. **判断对象所属的类**
```Java
//getClass()是在Object类中定义的，该方法返回的是一个Class对象，对象的getName()返回的是一个表示类名的字符串
public class Temp{
	public static void main(String[] args){
		Temp t = new Temp();
		System.out.println(t.getClass().getName());
	}
}

output:
Temp


/*
另一种检测方式是instance运算符，instanceof使用两个操作数：左边为对象的引用，右边是类名，表达式返回一个布尔值，
如果该对象是这种类或其子类的实例，为true，否则为false
*/
public class Temp{
	public static void main(String[] args){
		boolean check1 = "Texas" instanceof String;
		Object t = new Temp();
		boolean check2 = t instanceof String;
		System.out.println("check1:" + check1 + " check2:" + check2);
	}
}

output:
check1:true check2:false
```

3. **传参时使用++**
```Java
public class Temp{
	public static void main(String[] args){
		int i = 0;
		int j = testP(i++);
		System.out.println(j);
	}
	
	static int testP(int x){
		return x;
	}
}

output:
0

//结果表明，传参时使用++，需要注意，如果++在后，也是先传值，再自增
```
