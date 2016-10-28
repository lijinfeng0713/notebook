20161028  
## CSV读取写入之csvreader   

#### 1.概述   
&nbsp;&nbsp;&nbsp;&nbsp;csv是一种比较特殊的文件格式，以纯文本形式存储表格数据，数据之间多以逗号隔开。Java提供了一个操作CSV的工具——` csvreader `，使用csvreader需要导入<a href="jar/javacsv.jar">javacsv.jar</a>。    

#### 2.CsvWriter   
创建读入流   
```java  
CsvReader r = new CsvReader(INPUT_FILE_PATH, ',',Charset.forName("utf-8"));    
```    
构造函数` CsvReader() `有三个参数，第一个参数是String类型，是读入文件的路径；第二个参数是数据之间的分隔方式；第三个参数设置文件的编码。   

通过` getRawRecord() `函数获取每一行的数据，返回类型为String，即将每一行的数据存储在一个String类型的变量中。通过String类的` split() `方法，将每一行的数据分隔成一个数组，通过获取数组中的每个元素来获取csv中每一列的属性。    
```java   
String[] record = r.getRawRecord().split(",");   
```   
#### 3.CsvWriter   
创建写出流     
```java      
CsvWriter w = new CsvWriter(OUTPUT_FILE_PATH, ',',Charset.forName("utf-8"));   
```   
通过` writeRecord() ` 函数向指定的文件中写入数据，该函数的参数是一个String数组，注意在执行完该方法后要执行` flush() ` 函数来清除缓存。   
```java   
w.writeRecord(output.split(",")); 
w.flush();   
```   

注意，在执行完read和write操作后，需要关闭读入写出流。
