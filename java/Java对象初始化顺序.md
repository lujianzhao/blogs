### 例子一
```java
public class Test {

	public static Test t1 = new Test(1);
	public static Test t2 = new Test(2);

	public Test(int i) {
		System.out.println("构造方法" + i);
	}

	{
		System.out.println("构造块");
	}

	static {
		System.out.println("静态块");
	}

	public static void main(String[] args) {
		System.out.println("start");
		new Test(3);
		System.out.println("end");
	}
}

// 输出：
构造块
构造方法1
构造块
构造方法2
静态块
start
构造块
构造方法3
end
```
**所有static的执行顺序为从上到下**

http://blog.csdn.net/lanxuezaipiao/article/details/16753743

http://liujiacai.net/blog/2014/07/12/order-of-initialization-in-java/

http://www.importnew.com/21832.html
