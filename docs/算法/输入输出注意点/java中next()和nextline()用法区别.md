## next()

1. 停止录入的结束符有**空格**、**Tab键**和**回车键**（录入内容不含结束标志）
2. next（）对输入有效字符之前遇到的空格键、Tab键或Enter键等结束符，next（）方法会自动将其去掉，只有在输入有效字符之后，next（）方法才将其后输入的空格键、Tab键或Enter键等视为分隔符或结束符。所以next()不能得到带空格的字符串，而nextLine（）是遇到回车键才结束，所以可以得到带空格的字符串。

## nextline()

停止录入的结束标志只有回车键。

## 实例

- 输入“你好 世界”

```java
public class 键盘录入中next与nextline区别 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("输入“你好 世界”：");//1
        String str1 = sc.next();
        System.out.println(str1);//2
        System.out.println("输入“你好 世界”：");
        String str2 = sc.next();
        System.out.println(str2);//3
        System.out.println("输入“你好 世界”：");
        String str3 = sc.nextLine();
        System.out.println(str3);//4
    }
}
```

- 运行结果：

> 输入“你好 世界”：        //1   这里是提醒用户输入”你好 世界“   
> 你好 世界                    //       这里是用户仅有的一次输入，中间有空格，输入后，程序结束
> 你好                            //2     这里将输入的空格当作结束标志，所以只打印了空格前的”你好“
> 输入“你好 世界”：
> 世界                           //3     这里将“世界”之前的空格去掉，并将”世界“打印出来
> 输入“你好 世界”：          




以上数字4代码行没有打印，程序便结束了。因为第二个next()只接收了“世界”，而“世界”后的Enter键就被nextLine()接收。如果next()后面仅跟着一个nextLine()语句，那么可以再next()语句后面多加一个空的nextLine()，对于上面案例修改如下（黄色行）：

```java
Scanner sc = new Scanner(System.in);
System.out.println("输入“你好 世界”：");
String str1 = sc.next();
System.out.println(str1);
System.out.println("输入“你好 世界”：");
String str2 = sc.next();
System.out.println(str2);
sc.nextLine();
System.out.println("输入“你好 世界”：");
String str3 = sc.nextLine();
System.out.println(str3);
```

- 运行结果：

> 输入“你好 世界”：
> 你好 世界
> 你好
> 输入“你好 世界”：
> 世界
> 输入“你好 世界”：
> 你好 世界
> 你好 世界                            //4     


————————————————
版权声明：本文为CSDN博主「做难做的事」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_39593222/article/details/125157841