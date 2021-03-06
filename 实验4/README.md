<<<<<<< HEAD


### 1.获取URL地址并启动隐式Intent的调用



####  activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/url"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="输入网址"
        android:text="http://"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <Button
        android:onClick="toweb"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="浏览该网页"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toBottomOf="@id/url"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```



### MainActivity.java

```
package edu.finu.cse.myapplication10;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.content.Intent;
import android.net.Uri;
import android.view.View;
import android.widget.EditText;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {
    private EditText url;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        url= findViewById(R.id.url); //获取URL地址
    }

    public void toweb(View source){//页面视图的跳转
        Intent intent=new Intent();  //启动隐式Intent的调用
        intent.setAction(Intent.ACTION_VIEW);
        intent.setData(Uri.parse(url.getText().toString()));
        startActivity(intent);
    }

```



### 结果

![](./image/1.png)







### 2.自定义WebView来加载URL



#### activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/url"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="输入网址"
        android:text="http://"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <Button
        android:onClick="toweb"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="浏览该网页"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toBottomOf="@id/url"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```



#### MainActivity.java

```
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.content.Intent;
import android.net.Uri;
import android.view.View;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {
    private EditText url;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        url= findViewById(R.id.url); //获取URL地址
    }

    public void toweb(View source){//页面视图的跳转
        Intent intent=new Intent();  //隐式Intent的调用
        intent.setAction(Intent.ACTION_VIEW);
        intent.setData(Uri.parse(url.getText().toString()));
        Intent choose=Intent.createChooser(intent,"选择浏览器");
        startActivity(choose);
    }
}
```





### webview.java

```
import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;

import androidx.appcompat.app.AppCompatActivity;

public class webview extends AppCompatActivity {
    //自定义webview来加载URL
    private WebView web;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_webview);
        web = (WebView) findViewById(R.id.web);
        //接收URL网址
        String url=getIntent().getDataString();
        WebSettings webSettings=web.getSettings();
        webSettings.setJavaScriptEnabled(true);
        webSettings.setSupportZoom(true);
        webSettings.setAllowFileAccess(true);
        webSettings.setAllowContentAccess(true);
        //使用自定的web加载URL
        web.loadUrl(url);
        //使用自定的web打开网址
        web.setWebViewClient(new WebViewClient(){
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                web.loadUrl(url);
                return true;
            }
        });
    }
}
```







### androidmainfest.xml

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        android:usesCleartextTraffic="true" >
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".webview">
            <intent-filter>
                <action android:name="android.intent.action.VIEW"></action>
                <data android:scheme="http"/>
                <data android:scheme="https"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>

    </application>
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
```





### activity_webview.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    //定义webview视图
    <WebView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/web">
    </WebView>
</LinearLayout>
```



### 结果





![](./image/2.png)



![](./image/3.png)






=======


### 1.获取URL地址并启动隐式Intent的调用



####  activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/url"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="输入网址"
        android:text="http://"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <Button
        android:onClick="toweb"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="浏览该网页"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toBottomOf="@id/url"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```



### MainActivity.java

```
package edu.finu.cse.myapplication10;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.content.Intent;
import android.net.Uri;
import android.view.View;
import android.widget.EditText;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {
    private EditText url;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        url= findViewById(R.id.url); //获取URL地址
    }

    public void toweb(View source){//页面视图的跳转
        Intent intent=new Intent();  //启动隐式Intent的调用
        intent.setAction(Intent.ACTION_VIEW);
        intent.setData(Uri.parse(url.getText().toString()));
        startActivity(intent);
    }

```



### 结果

![](./image/1.png)







### 2.自定义WebView来加载URL



#### activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/url"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="输入网址"
        android:text="http://"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <Button
        android:onClick="toweb"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="浏览该网页"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toBottomOf="@id/url"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```



#### MainActivity.java

```
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.content.Intent;
import android.net.Uri;
import android.view.View;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {
    private EditText url;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        url= findViewById(R.id.url); //获取URL地址
    }

    public void toweb(View source){//页面视图的跳转
        Intent intent=new Intent();  //隐式Intent的调用
        intent.setAction(Intent.ACTION_VIEW);
        intent.setData(Uri.parse(url.getText().toString()));
        Intent choose=Intent.createChooser(intent,"选择浏览器");
        startActivity(choose);
    }
}
```





### webview.java

```
import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;

import androidx.appcompat.app.AppCompatActivity;

public class webview extends AppCompatActivity {
    //自定义webview来加载URL
    private WebView web;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_webview);
        web = (WebView) findViewById(R.id.web);
        //接收URL网址
        String url=getIntent().getDataString();
        WebSettings webSettings=web.getSettings();
        webSettings.setJavaScriptEnabled(true);
        webSettings.setSupportZoom(true);
        webSettings.setAllowFileAccess(true);
        webSettings.setAllowContentAccess(true);
        //使用自定的web加载URL
        web.loadUrl(url);
        //使用自定的web打开网址
        web.setWebViewClient(new WebViewClient(){
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                web.loadUrl(url);
                return true;
            }
        });
    }
}
```







### androidmainfest.xml

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        android:usesCleartextTraffic="true" >
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".webview">
            <intent-filter>
                <action android:name="android.intent.action.VIEW"></action>
                <data android:scheme="http"/>
                <data android:scheme="https"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>

    </application>
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
```





### activity_webview.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    //定义webview视图
    <WebView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/web">
    </WebView>
</LinearLayout>
```



### 结果





![](./image/2.png)



![](./image/3.png)






>>>>>>> “irst commit
