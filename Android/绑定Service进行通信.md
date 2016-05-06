###### 201605006  

## 绑定Service进行通信  
Service的使用有两种模式，一种是启动模式，一种是绑定模式。因此绑定Service进行通信也对应的有两种模式，即通过启动模式绑定数据和通过绑定模式绑定数据。   

#### 1 启动模式绑定数据   
###### activity_main.xml   
```html   
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.ljf_.app2.MainActivity"
    android:orientation="vertical">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/edit01"
        android:text="默认信息"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnStartService"
        android:text="启动服务"
        />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnStopService"
        android:text="停止服务"/>
</LinearLayout>  
```  
###### MainActivity.java  
```java   
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private EditText editText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editText = (EditText) findViewById(R.id.edit01);
        //加载按钮控件
        findViewById(R.id.btnStartService).setOnClickListener(this);
        findViewById(R.id.btnStopService).setOnClickListener(this);


    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.btnStartService :
                Intent intent = new Intent(MainActivity.this, MyService.class);
                intent.putExtra("data", editText.getText().toString());
                startService(intent);
                break;

            case R.id.btnStopService :
                stopService(new Intent(MainActivity.this, MyService.class));
                break;

        }
    }

}
```   

###### MyService.java   
```java  
public class MyService extends Service {

    private boolean RUNNING = false;
    private String data = "默认信息";
    public MyService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
        // TODO: Return the communication channel to the service.
        return new Binder();
    }

    @Override
    public void onCreate() {
        super.onCreate();
        RUNNING = true;

        new Thread() {
            @Override
            public void run() {
                super.run();
                while (RUNNING) {
                    System.out.println(data);
                    try {
                        sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        data = intent.getStringExtra("data");
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        RUNNING = false;
    }
}

```   
在这种模式中是通过启动服务来实现数据的同步的，其中` onCreate() ` 方法只执行一次，即只创建一个Service的实例。每点击一次启动服务按钮，` onStartCommand() ` 方法就会执行一次，通过这个方法来更行data字符串的内容。   

#### 2 绑定模式实现数据绑定   
###### activity_main.xml   
```html  
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.ljf_.app2.MainActivity"
    android:orientation="vertical">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/edit01"
        android:text="默认信息"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnBindService"
        android:text="绑定服务"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnUnbindService"
        android:text="解除绑定服务"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnSyncData"
        android:text="同步数据"/>
</LinearLayout>
```  

###### MainActivity.java  
```java  
public class MainActivity extends AppCompatActivity implements View.OnClickListener, ServiceConnection {

    private EditText editText;
    private MyService.Binder binder;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editText = (EditText) findViewById(R.id.edit01);
        //加载按钮控件
        findViewById(R.id.btnBindService).setOnClickListener(this);
        findViewById(R.id.btnUnbindService).setOnClickListener(this);
        findViewById(R.id.btnSyncData).setOnClickListener(this);


    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.btnBindService :
                bindService(new Intent(this, MyService.class), this, Context.BIND_AUTO_CREATE);
                break;

            case R.id.btnUnbindService :
                unbindService(this);
                break;

            case R.id.btnSyncData :
                if (binder != null) {
                    binder.setData(editText.getText().toString());
                }
        }
    }

    @Override
    public void onServiceConnected(ComponentName name, IBinder service) {
        binder = (MyService.Binder) service;
    }

    @Override
    public void onServiceDisconnected(ComponentName name) {

    }
}
```  

###### MyService.java   
```java   
public class MyService extends Service {

    private boolean RUNNING = false;
    private String data = "默认信息";
    public MyService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
        // TODO: Return the communication channel to the service.
        return new Binder();
    }

    public class Binder extends android.os.Binder {
        public void setData (String data) {
            MyService.this.data = data;
        }
    }

    @Override
    public void onCreate() {
        super.onCreate();
        RUNNING = true;

        new Thread() {
            @Override
            public void run() {
                super.run();
                while (RUNNING) {
                    System.out.println(data);
                    try {
                        sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        RUNNING = false;
    }
}
```  
在这种方式中，是先通过点击绑定服务按钮来绑定服务，在通过点击同步数据按钮来调用` setData() `方法来实现数据的同步。
