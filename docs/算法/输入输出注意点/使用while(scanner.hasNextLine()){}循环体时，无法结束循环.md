## 问题

当需要从控制台中读取数据时，发现当使用while(scanner.hasNextLine()){}时，发现无法结束循环体。在网上找了一下原因。说是因为System.in的数据流还没有关闭，所以scanner会一直等在下一行的数据的到来，所以不管回车键按了多少次都不会跳出循环体。

```java
public class Test {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		StringBuilder sb = new StringBuilder();
		while(scanner.hasNextLine()){   
		//按照api上说的，还有下一行的时候就会返回true，
		//那应该其他情况返回的是false；但是当多次回车之后，循环体还是不会退出。
			String s = scanner.nextLine();
			sb = sb.append(s);
		}
		System.out.println(sb.toString());//这句话永远不会的被执行。
	}

}
```

## 那么怎样改进呢？大致有两种方式；

1、在循环体内增加判断条件，在内部控制跳出循环体。但是这种方法需要连续按两次回车键才能结束循环体。


			
	public class Test {
		public static void main(String[] args) {
			Scanner scanner = new Scanner(System.in);
			StringBuilder sb = new StringBuilder();
			while(scanner.hasNextLine()){
	String s = scanner.nextLine();
			if(s.equals("")) break;//""里面没有空格号；这里的equals也不能换成==；
			sb = sb.append(s);
		}
		System.out.println(sb.toString());
	}

}
运行结果：
sfgfgdfgd
sdfsgdfds
                   //这里需要两次回车键
sfgfgdfgdsdfsgdfds

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
2、当使用的是hasNext（）一系列的方法时，可以增加结束符来控制循环体的结束：

public class Test {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		StringBuilder sb = new StringBuilder();
		while(!scanner.hasNext("#")){  //这个控制符如果使用$，居然也不会跳出循环体，我也不知道为啥。
			String s = scanner.next();
			sb = sb.append(s);
			}
		System.out.println(sb.toString());
	}

}
运行结果：
sfsdfsg sfsdgg
sfsgggf sfs
#
sfsdfsg sfsdgg sfsgggf sfs 
---
版权声明：本文为CSDN博主「一叶扁舟在大海」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zixuexiaobaihu/article/details/108660906