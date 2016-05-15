###### 20160515   

## 单选和复选组件   

在单选组件RadioButton中，通过` isChecked() ` 方法来判断按钮的选中情况。复选组件CheckBox则需要通过复写` CompoundButton.OnCheckedChangeListener ` 的` onCheckedChanged `方法来监听复选按钮的选择事件。以下给出两个组件的实现实例。   

##### 单选组件   
######  布局文件   
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
    android:orientation="vertical"
    tools:context="com.lijinfeng.select.RadioActivity">

    <RadioGroup
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        >
        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/raA"
            android:text="中国"/>
        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/raB"
            android:text="俄罗斯"/>
        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/raC"
            android:text="日本"/>
        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/raD"
            android:text="美国"/>
    </RadioGroup>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnSubmit"
        android:text="提交"/>
</LinearLayout>
```   
###### Activity   
```java   
public class RadioActivity extends AppCompatActivity {

    private RadioButton raA;
    private RadioButton raB;
    private RadioButton raC;
    private RadioButton raD;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_radio);

        raA = (RadioButton) findViewById(R.id.raA);
        raB = (RadioButton) findViewById(R.id.raB);
        raC = (RadioButton) findViewById(R.id.raC);
        raD = (RadioButton) findViewById(R.id.raD);

        findViewById(R.id.btnSubmit).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (raA.isChecked()) {
                    Toast.makeText(RadioActivity.this,"中国", Toast.LENGTH_SHORT).show();
                }
                if (raB.isChecked()) {
                    Toast.makeText(RadioActivity.this,"俄罗斯", Toast.LENGTH_SHORT).show();
                }
                if (raC.isChecked()) {
                    Toast.makeText(RadioActivity.this,"日本", Toast.LENGTH_SHORT).show();
                }
                if (raD.isChecked()) {
                    Toast.makeText(RadioActivity.this,"美国", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
}
```    

##### 复选组件   
###### 布局文件   
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
    android:orientation="vertical"
    tools:context="com.lijinfeng.select.CheckActivity">

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/checkbox1"
        android:text="中国"/>

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/checkbox2"
        android:text="俄罗斯"/>

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/checkbox3"
        android:text="美国"/>

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/checkbox4"
        android:text="日本"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/select"/>
</LinearLayout>
```     

###### Activity   
```java   
public class CheckActivity extends AppCompatActivity implements CompoundButton.OnCheckedChangeListener{

    private CheckBox option1,option2,option3,option4;
    private TextView selectOption;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_check);

        selectOption = (TextView) findViewById(R.id.select);

        option1 = (CheckBox) findViewById(R.id.checkbox1);
        option2 = (CheckBox) findViewById(R.id.checkbox2);
        option3 = (CheckBox) findViewById(R.id.checkbox3);
        option4 = (CheckBox) findViewById(R.id.checkbox4);

        option1.setOnCheckedChangeListener(this);
        option2.setOnCheckedChangeListener(this);
        option3.setOnCheckedChangeListener(this);
        option4.setOnCheckedChangeListener(this);

    }

    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {

        String result = "选择了：";
        if (option1.isChecked()) {
            result += option1.getText()+";";
            selectOption.setText(result);
        }
        if (option2.isChecked()) {
            result += option2.getText()+";";
            selectOption.setText(result);
        }
        if (option3.isChecked()) {
            result += option3.getText()+";";
            selectOption.setText(result);
        }
        if (option4.isChecked()) {
            result += option4.getText();
            selectOption.setText(result);
        }
    }
}
```   