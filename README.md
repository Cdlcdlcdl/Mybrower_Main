# Mybrower_Main
主页面

```

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.example.pc.a5.MainActivity">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        >
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="20sp"
            android:hint="输入网址"
            android:layout_weight="1"
            android:id="@+id/et_url"
            />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="访问"
            android:id="@+id/btn_go"
            />
    </LinearLayout>
    
    <WebView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/my_webView"
        >

    </WebView>
</LinearLayout>

```

添加按钮监听器

```

package com.example.pc.a5;

import android.content.Intent;
import android.net.Uri;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity implements View.OnClickListener{

    private Button btn_go;
    private EditText et_url;
    private String urlHead="https://";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
    }

    private void initView() {
        btn_go= (Button) findViewById(R.id.btn_go);
        et_url= (EditText) findViewById(R.id.et_url);
        btn_go.setOnClickListener(this);

    }

    @Override
    public void onClick(View view) {
        switch (view.getId()){
            case R.id.btn_go:
                Intent sendIntent = new Intent();
                sendIntent.setAction(Intent.ACTION_VIEW);
                String url=et_url.getText().toString();
                sendIntent.setData(Uri.parse(urlHead+url));
                if (sendIntent.resolveActivity(getPackageManager()) != null) {
                    startActivity(sendIntent);
                }
                break;

        }
    }
}

```

在浏览器端

```
package com.example.pc.mybrowser;

import android.net.Uri;
import android.os.Bundle;
import android.os.PersistableBundle;
import android.support.annotation.Nullable;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;



public class MyWebView extends AppCompatActivity {

    private WebView webView;
    private String Url;


    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
        setWebView();
    }

    private void initView() {
        webView= (WebView) findViewById(R.id.webView);
        Uri url=getIntent().getData();
        if(url==null){
            Url="https://www.baidu.com";
        }
        else {
            Url=url.toString();
        }
        Log.e("asd ",Url);
    }


    private void setWebView() {
        WebSettings webSettings=webView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        webSettings.setSupportZoom(true);

        webView.loadUrl(Url);
        webView.setWebViewClient(new WebViewClient(){

            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                webView.loadUrl(Url);
                return true;
            }
        });
    }
}

```

修改Androidmainfest.xml

```<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.pc.mybrowser">


    <uses-permission  android:name="android.permission.INTERNET"/>
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MyWebView">
            <intent-filter>

                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

            <intent-filter>

                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE"/>

                <data android:scheme="http"/>
                <data android:scheme="https"/>
            </intent-filter>

        </activity>
    </application>

</manifest>

```



截图

![](https://i.loli.net/2019/04/17/5cb6872b5a2fd.png)

点击访问

![](https://i.loli.net/2019/04/17/5cb68747aa9a2.png)

成功访问

![](https://i.loli.net/2019/04/17/5cb68778ab491.png)



