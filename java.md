# Java 学习笔记

### 遍历map的三种方法

##### 方法一：利用foreach遍历key，再通过get方法根据key来获取value

``` java 
public class MapTest {
	
	public static void main(String[] args) {
		
		Map<String,String> map = new HashMap<String,String>();
		
		map.put("xiaoming", "22");
		map.put("xiaohua", "24");
		
		if(map.isEmpty()) {
			System.out.println("This map is empty！");
		} else {
			for (String key:map.keySet()) {
				System.out.println("name is :" + key);
				System.out.println("age is :" + map.get(key));
			}
		}
	}
}

```

##### 方法二：利用Iterator来遍历

``` java
public class MapTest {
	
	public static void main(String[] args) {
		
		Map<String,String> map = new HashMap<String,String>();
		
		map.put("xiaoming", "22");
		map.put("xiaohua", "24");
		
		if(map.isEmpty()) {
			System.out.println("This map is empty！");
		} else {
			Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
			while (it.hasNext()) {
			  Map.Entry<String, String> entry = it.next();
			  System.out.println("name ： " + entry.getKey() + "； age is " + entry.getValue());
			}

		}
	}
}

```

##### 方法三：利用map的entrySet来遍历key和value（大容量是推荐使用此方法）

```java 
public class MapTest {
	
	public static void main(String[] args) {
		
		Map<String,String> map = new HashMap<String,String>();
		
		map.put("xiaoming", "22");
		map.put("xiaohua", "24");
		
		if(map.isEmpty()) {
			System.out.println("This map is empty！");
		} else {
			for (Map.Entry<String, String> entry : map.entrySet()) {
				   System.out.println("name： " + entry.getKey() + "； age is " + entry.getValue());
				  }
		}
	}
}
```  
------------------------------------------------2016-02-24---------------------------------------------------------  
### Java输入输出流  
#### 1 Java I/O机制
&nbsp;&nbsp;&nbsp;&nbsp;Java中I/O操作主要是指使用Java进行输入，输出操作。Java所有的I/O机制都是基于数据流进行输入输出，这些数据流表示了字符或者字节数据的流动序列。Java的I/O流提供了读写数据的标准方法。任何Java中表示数据源的对象都会提供以数据流的方式读写它的数据的方法。  
&nbsp;&nbsp;&nbsp;&nbsp;流IO的好处是简单易用，缺点是效率较低。块IO效率很高，但编程比较复杂。 
##### Java IO模型  :
&nbsp;&nbsp;&nbsp;&nbsp;Java的IO模型设计非常优秀，它使用Decorator模式，按功能划分Stream，可以动态装配这些Stream，以便获得所需要的功能。例如，当我们需要一个具有缓冲的文件输入流，则应当组合使用FileInputStream和BufferedInputStream。  
##### Java IO包括  :  
&nbsp;&nbsp;&nbsp;&nbsp;标准输入输出，文件的操作，网络上的数据流，字符串流，对象流，zip文件流等等。将数据冲外存中读取到内存中的称为输入流，将数据从内存写入外存中的称为输出流。  
#### 2 标准IO  
##### 2.1 标准输入、输出数据流  
&nbsp;&nbsp;&nbsp;&nbsp; java系统自带的标准数据流： ` java.lang.System `  
	
```java 
    public final class System  extends Object{   
       static  PrintStream  err;//标准错误流（输出）  
       static  InputStream  in;//标准输入(键盘输入流)  
       static  PrintStream  out;//标准输出流(显示器输出流)  
    }  
 ```  
##### 注意
 * System类不能创建对象，只能直接使用它的三个静态成员
 * 每当main方法被执行时,就自动生成上述三个对象     
 
##### 2.1.1 标准输入流 System.in   
&nbsp;&nbsp;&nbsp;&nbsp; 	System.in读取标准输入设备数据（从标准输入获取数据，一般是键盘），其数 据类型为InputStream。方法：  
&nbsp;&nbsp;&nbsp;&nbsp; ` int read() `  //返回ASCII码。若,返回值=-1，说明没有读取到任何字节读取工作结束。  
&nbsp;&nbsp;&nbsp;&nbsp; ` int read(byte[] b) `  //读入多个字节到缓冲区b中返回值是读入的字节数  
		
```java 
public class StandardInputOutput {  
    public static void main(String args[]) {  
        int b;  
        try {  
            System.out.println("please Input:");  
            while ((b = System.in.read()) != -1) {  
                System.out.print((char) b);  
            }  
        } catch (IOException e) {  
            System.out.println(e.toString());  
        }  
    }  
}   
```  
##### 2.1.2 标准输入流 System.out    
&nbsp;&nbsp;&nbsp;&nbsp; System.out向标准设备输出数据，其数据类型为PrintStream。方法：  
&nbsp;&nbsp;&nbsp;&nbsp; ` void print(参数) `  
&nbsp;&nbsp;&nbsp;&nbsp; ` void println(参数) `  

&nbsp;&nbsp;&nbsp;&nbsp;标准输出通过System.out调用println方法输出参数并换行，而print方法输出参数但不换行。println或print方法都通过重载实现了输出基本数据类型的多个方法，包括输出参数类型为boolean、char、int、long、float和double。同时，也重载实现了输出参数类型为char[]、String和Object的方法。其中，print（Object）和println（Object）方法在运行时将调 用参数Object的toString方法。  
```java 
public class StandardInputOutput {  
    public static void main(String args[]) {  
        String s;  
        // 创建缓冲区阅读器从键盘逐行读入数据  
        InputStreamReader ir = new InputStreamReader(System.in);  
        BufferedReader in = new BufferedReader(ir);  
        System.out.println("Unix系统: ctrl-d 或 ctrl-c 退出"  
                + "\nWindows系统: ctrl-z 退出");  
        try {  
            // 读一行数据，并标准输出至显示器  
            s = in.readLine();  
            // readLine()方法运行时若发生I/O错误，将抛出IOException异常  
            while (s != null) {  
                System.out.println("Read: " + s);  
                s = in.readLine();  
            }  
            // 关闭缓冲阅读器  
            in.close();  
        } catch (IOException e) { // Catch any IO exceptions.  
            e.printStackTrace();  
        }  
    }  
}   
```  

##### 2.1.3 标准错误流 System.err  

&nbsp;&nbsp;&nbsp;&nbsp;System.err输出标准错误，其数据类型为PrintStream。可查阅API获得详细说明  

##### 2.2 File文件流  
##### 2.2.1 文件写入  
&nbsp;&nbsp;&nbsp;&nbsp;我们都知道，Java IO体系中分为流式部分和非流式部分，其中流式部分又分为字节流和字符流两种。因此，文件的写入至少有两种方法
##### 方法一：FileWriter（字符流）  
```java  
import java.io.*;

/**
 * Created by ljf-梁燕双栖 on 2016/2/25.
 */
public class FileStreamTest {
    public static void main(String args[]) {
        String s = "hello world";
        try {
            FileWriter fo = new FileWriter("D:/Log/out.txt");
            fo.write(s);
            fo.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```  
&nbsp;&nbsp;&nbsp;&nbsp; 这种方法中可以看出是将文件写入到了文本文件out.txt中，倘若该路径下没有这个文件，首先会生成这个文件，然后再写入。在执行` write() ` 方法后，最好要调用` flush() ` 方法，避免缓存问题（此问题我已经尝试过）。  
##### 方法二：FileOutputStream（字节流）  
```java  
import java.io.*;

/**
 * Created by ljf-梁燕双栖 on 2016/2/25.
 */
public class FileStreamTest {
    public static void main(String args[]) {
        String s = "hello world!";
        try {
            FileOutputStream fo = new FileOutputStream("D:/Log/out.txt");
            fo.write(s.getBytes());
            fo.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```  
&nbsp;&nbsp;&nbsp;&nbsp; 由于FileOutputStream是通过字节流进行写入，所以我们很容易看到了方法二中` write() `与方法一中的区别。  
