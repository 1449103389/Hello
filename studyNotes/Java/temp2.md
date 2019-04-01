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
