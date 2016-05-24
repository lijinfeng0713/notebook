###### 20160524   

## SnackBar信息提示    
&nbsp;&nbsp;&nbsp;&nbsp;在安卓中，给用户显示信息的方式有很多种，其中SnackBar可以给用户提供一种快速的、弹出式的消息显示方式。当SnackBar显示的时候，当前的Activity依旧是可见的、可交互的，而且在短暂的显示后，SnackBar将会自动消失。  
&nbsp;&nbsp;&nbsp;&nbsp; SnackBar类取代了传统的Toast，尽管目前Toast仍在被使用，但SnackBar是给用户显示简短信息的一种首选方式。    

#### 1、使用SnackBar    
需要注意的是，在Android5.0以下的版本中使用SnackBar时，需要在gradle文件的依赖中添加` compile 'com.android.support:design:23.3.0' `   
一个SnackBar需要附加到视图上。如果它是连接到来自视图类的任何对象，如任何常用的布局对象，那么snackbar只能提供基本功能。然而，如果snackbar是连接到一个coordinatorlayout的snackbar，则可以获得额外的功能。<a href="https://developer.android.com/training/snackbar/showing.html" title="">参考文档</a>    
官方提供的显示效果     <a href="https://developer.android.com/images/training/snackbar/snackbar_button_move.mp4" title="">官方效果</a>    

###### 使用了CoordinatorLayout的布局文件    
```html   
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    android:id="@+id/myCoordinatorLayout"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- Here are the existing layout elements, now wrapped in
         a CoordinatorLayout -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btnShowSnackbar"
            android:text="显示"/>

    </LinearLayout>

</android.support.design.widget.CoordinatorLayout>
```   
注意：在这个布局文件中，必须为CoordinatorLayout添加一个id，后面显示SnackBar时将会使用到。    

###### 创建SnackBar    
```java   
Snackbar mySnackbar = Snackbar.make(viewId, stringId, duration);   
```  
* viewId: 用于显示SnackBar的view的id，即上边提到的id    
* stringId： 要显示的文本内容     
* duration： 显示时长，取值为` Snackbar.LENGTH_SHORT ` 或 ` Snackbar.LENGTH_LONG `    

```java   
findViewById(R.id.btnShowSnackbar).setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Snackbar.make(findViewById(R.id.myCoordinatorLayout), "显示snackbar",
                Snackbar.LENGTH_SHORT)
                .show();
    }
});   
```   


