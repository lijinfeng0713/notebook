20160326  
## 单例模式   
#### 1 概述  
&nbsp;&nbsp;&nbsp;&nbsp;Singleton模式主要作用是保证在Java程序中，一个类只有一个实例存在。在很多操作中，比如建立目录 数据库连接都需要这样的单线程操作。   
&nbsp;&nbsp;&nbsp;&nbsp;还有，singleton能够被状态化；这样，多个单例类在一起就可以作为一个状态仓库一样向外提供服务。
另外方面，Singleton也能够被无状态化。提供工具性质的功能。  
&nbsp;&nbsp;&nbsp;&nbsp;Singleton模式就为我们提供了这样实现的可能。使用Singleton的好处还在于可以节省内存，因为它限制了实例的个数，有利于Java垃圾回收（garbage collection）。我们常常看到工厂模式中类装入器(class loader)中也用Singleton模式实现的，因为被装入的类实际也属于资源。  
#### 2.1 单例模式三要点  
*某个类只能有一个实例  
*它必须自行创建这个实例  
*它必须自行向整个系统提供这个实例。  

#### 2.2 单例模式实现  
* 创建静态私有对象  
* 构造方法私有化  
* 提供静态的获取实例的方法  

##### 2.1  饿汉式单例模式  
&nbsp;&nbsp;&nbsp;&nbsp;饿汉模式是在定义静态变量的时候实例化单例类，因此在类加载的时候就已经创建了单例类对象  
```java  
package pattern;

/**Description : 饿汉式单例模式
 * Created by ljf-梁燕双栖 on 2016/3/26.
 */
public class Singleton {
    private static Singleton instance = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return instance;
    }
}
```  
##### 2.2 懒汉式单例模式  
&nbsp;&nbsp;&nbsp;&nbsp;懒汉式实现方式是在第一次调用` getInstance() `方法时进行实例化，在类加载时并不进行实例化，这种技术也叫延迟加载（Lazy Load）技术，即在需要的时候再加载实例。为了避免多个线程同时调用` getInstance() `方法，我们使用synchronized来实现同步。  
```java  
package pattern;

/**Description : 懒汉式单例模式
 * Created by ljf-梁燕双栖 on 2016/3/26.
 */
public class LazySingleton {
    private static LazySingleton instance = null;
    private LazySingleton() {}
    public static synchronized LazySingleton getInstance() {
        if (instance == null) {
            instance =  new LazySingleton();
        }
        return instance;
    }
}
```  
&nbsp;&nbsp;&nbsp;&nbsp;该懒汉式单例类在getInstance()方法前面增加了关键字synchronized进行线程锁，以处理多个线程同时访问的问题。但是，上述代码虽然解决了线程安全问题，但是每次调用getInstance()时都需要进行线程锁定判断，在多线程高并发访问环境中，将会导致系统性能大大降低。如何既解决线程安全问题又不影响系统性能呢？我们继续对懒汉式单例进行改进。事实上，我们无须对整个getInstance()方法进行锁定，只需对其中的代码“instance = new LazySingleton();”进行锁定即可。因此getInstance()方法可以进行如下改进：  
##### 2.3 改进懒汉式  
```java  
package pattern;

/**Description : 懒汉式单例模式
 * Created by ljf-梁燕双栖 on 2016/3/26.
 */
public class LazySingleton {
    private static LazySingleton instance = null;
    private LazySingleton() {}
    public static LazySingleton getInstance() {
        if (instance == null) {
            synchronized(LazySingleton.class) {
                instance =  new LazySingleton();
            }
        }
        return instance;
    }
}
```  
&nbsp;&nbsp;&nbsp;&nbsp;以上改进似乎解决了问题，但是仍然可能导致单例对象不唯一。当有两个线程A和B同时执行getInstance方法时，都可以通过if判断，由于使用了synchronized实现同步,所以当线程A执行synchronized修饰的创建实例对象的语句时，线程B处于等待状态，只有当A执行完B才可以执行synchronized锁定的代码。但当A执行完时，线程B并不知道实例对象已经被创建，所以仍会执行锁定的内容，从而会导致单例对象不唯一。所以需要继续改进。  
##### 2.4 双重检验锁定  
```java  
package pattern;

/**Description : 懒汉式单例模式
 * Created by ljf-梁燕双栖 on 2016/3/26.
 */
public class LazySingleton {
    private volatile static LazySingleton instance;
    private LazySingleton() {}
    public static LazySingleton getInstance() {
        if (instance == null) {
            synchronized(LazySingleton.class) {
                if (instance == null)
                    instance =  new LazySingleton();
            }
        }
        return instance;
    }
}
```  
&nbsp;&nbsp;&nbsp;&nbsp;本次改进中，改进了两个地方，除了再次对instance对象判断外，还需要用volatile关键字来修饰静态的instance对象，以确保多个线程都能正确的处理。由于volatile关键字会屏蔽Java虚拟机所做的一些代码优化，可能会导致系统运行效率降低，所以这种方式也不是最完美的实现方式。  

##### 饿汉式和懒汉式比较  
&nbsp;&nbsp;&nbsp;&nbsp;饿汉式单例类在类被加载时就将自己实例化，它的优点在于无须考虑多线程访问问题，可以确保实例的唯一性；从调用速度和反应时间角度来讲，由于单例对象一开始就得以创建，因此要优于懒汉式单例。但是无论系统在运行时是否需要使用该单例对象，由于在类加载时该对象就需要创建，因此从资源利用效率角度来讲，饿汉式单例不及懒汉式单例，而且在系统加载时由于需要创建饿汉式单例对象，加载时间可能会比较长。  
&nbsp;&nbsp;&nbsp;&nbsp;懒汉式单例类在第一次使用时创建，无须一直占用系统资源，实现了延迟加载，但是必须处理好多个线程同时访问的问题，特别是当单例类作为资源控制器，在实例化时必然涉及资源初始化，而资源初始化很有可能耗费大量时间，这意味着出现多线程同时首次引用此类的机率变得较大，需要通过双重检查锁定等机制进行控制，这将导致系统性能受到一定影响。  
##### 2.5 静态内部类实现方式  
&nbsp;&nbsp;&nbsp;&nbsp;实现了延迟加载同时也保证了线程安全。  
```java  
package pattern;

/**Description: 静态内部类方式实现单例模式
 * Created by ljf-梁燕双栖 on 2016/3/26.
 */
public class Singleton3 {
    private static class SingletonLoader {
        private static final Singleton3 instance = new Singleton3();
    }
    private Singleton3() {}
    public static Singleton3 getInstance() {
        return SingletonLoader.instance;
    }
}
```  
###### 2.6 枚举实现方式  
```java  
public enum Singleton {
    INSTANCE;
    public void whateverMethod() {
    }
}  
```  

