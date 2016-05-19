###### 20160519  

## ActionBar的使用    
&nbsp;&nbsp;&nbsp;&nbsp;作为Android 3.0之后引入的新的对象，ActionBar可以说是一个方便快捷的导航神器。它可以作为活动的标题，突出活动的一些关键操作（如“搜索”、“创建”、“共享”等）、作为菜单的灵活使用，还可以实现类似TabWidget的标签功能以及下拉导航的功能，系统能够很好根据不同的屏幕配置来适应ActionBar的外观，配合起Fragement可谓是十分强大。   

可以通过menu.xml文件往ActionBar中添加按钮等组件，具体例子如下：   
###### menu.xml文件   
```html   
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto" >
    <item android:id="@+id/action_alarm"
        android:title="@string/edit"
        android:icon="@android:drawable/ic_lock_idle_alarm"
        app:showAsAction="ifRoom"/>
    <item android:id="@+id/action_camera"
        android:title="@string/save"
        android:icon="@android:drawable/ic_menu_camera"
        app:showAsAction="always"/>
</menu>
```   
###### 加载menu文件   
```java   
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu, menu);
        return super.onCreateOptionsMenu(menu);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.action_alarm:
                Toast.makeText(MainActivity.this,"点击了闹钟按钮",Toast.LENGTH_SHORT).show();
            case R.id.action_camera:
                Toast.makeText(MainActivity.this,"点击了相机按钮",Toast.LENGTH_SHORT).show();
            default:
                return super.onOptionsItemSelected(item);
        }
    }
}
```     
![](img/2016051903.jpg)  


在menu.xml文件中有个属性很关键即showAsAction，该属性共有五个属性值，如下：
* ` ifRoom ` : 会显示在Item中，但是如果已经有4个或者4个以上的Item时会隐藏在溢出列表中。当然个
数并不仅仅局限于4个，依据屏幕的宽窄而定     
* ` never ` : 永远不会显示。只会在溢出列表中显示，而且只显示标题，所以在定义item的时候，最好
把标题都带上    
* ` always ` : 无论是否溢出，总会显示   
* ` withText ` : withText值示意Action bar要显示文本标题。Action bar会尽可能的显示这个
标题，但是，如果图标有效并且受到Action bar空间的限制，文本标题有可
能显示不全     
* ` collapseActionView `: 声明了这个操作视窗应该被折叠到一个按钮中，当用户选择这个按钮时，这个操作视窗展开。否则，
这个操作视窗在默认的情况下是可见的，并且即便在用于不适用的时候，也要占据操作栏的有效空间。
一般要配合ifRoom一起使用才会有效果    



