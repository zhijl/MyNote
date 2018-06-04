# Android 开发笔记

## AsyncTask 的使用

在Android中实现异步任务机制有两种方式，Handler和AsyncTask。

Handler模式需要为每一个任务创建一个新的线程，任务完成后通过Handler实例向UI线程发送消息，完成界面的更新，这种方式对于整个过程的控制比较精细，但也是有缺点的，例如代码相对臃肿，在多个任务同时执行时，不易对线程进行精确的控制。

为了简化操作，Android1.5提供了工具类android.os.AsyncTask，它使创建异步任务变得更加简单，不再需要编写任务线程和Handler实例即可完成相同的任务。

### AsyncTask 定义

三种泛型类型分别代表“启动任务执行的输入参数”、“后台任务执行的进度”、“后台计算结果的类型”。在特定场合下，并不是所有类型都被使用，如果没有被使用，可以用java.lang.Void类型代替。

一个异步任务的执行一般包括以下几个步骤：

1. execute(Params... params)，执行一个异步任务，需要我们在代码中调用此方法，触发异步任务的执行。

2. onPreExecute()，在execute(Params... params)被调用后立即执行，一般用来在执行后台任务前对UI做一些标记。

3. doInBackground(Params... params)，在onPreExecute()完成后立即执行，用于执行较为费时的操作，此方法将接收输入参数和返回计算结果。在执行过程中可以调用publishProgress(Progress... values)来更新进度信息。

4. onProgressUpdate(Progress... values)，在调用publishProgress(Progress... values)时，此方法被执行，直接将进度信息更新到UI组件上。

5. onPostExecute(Result result)，当后台操作结束时，此方法将会被调用，计算结果将做为参数传递到此方法中，直接将结果显示到UI组件上。

在使用的时候，有几点需要格外注意：

1. 异步任务的实例必须在UI线程中创建。

2. execute(Params... params)方法必须在UI线程中调用。

3. 不要手动调用onPreExecute()，doInBackground(Params... params)，onProgressUpdate(Progress... values)，onPostExecute(Result result)这几个方法。

4. 不能在doInBackground(Params... params)中更改UI组件的信息。

5. 一个任务实例只能执行一次，如果执行第二次将会抛出异常。

### 示例代码

``` java
package com.scott.async;  
  
import java.io.ByteArrayOutputStream;  
import java.io.InputStream;  
  
import org.apache.http.HttpEntity;  
import org.apache.http.HttpResponse;  
import org.apache.http.HttpStatus;  
import org.apache.http.client.HttpClient;  
import org.apache.http.client.methods.HttpGet;  
import org.apache.http.impl.client.DefaultHttpClient;  
  
import android.app.Activity;  
import android.os.AsyncTask;  
import android.os.Bundle;  
import android.util.Log;  
import android.view.View;  
import android.widget.Button;  
import android.widget.ProgressBar;  
import android.widget.TextView;  
  
public class MainActivity extends Activity {  
  
    private static final String TAG = "ASYNC_TASK";  
  
    private Button execute;  
    private Button cancel;  
    private ProgressBar progressBar;  
    private TextView textView;  
  
    private MyTask mTask;  
  
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.main);  
  
        execute = (Button) findViewById(R.id.execute);  
        execute.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                //注意每次需new一个实例,新建的任务只能执行一次,否则会出现异常  
                mTask = new MyTask();  
                mTask.execute("http://www.baidu.com");  
  
                execute.setEnabled(false);  
                cancel.setEnabled(true);  
            }  
        });  
        cancel = (Button) findViewById(R.id.cancel);  
        cancel.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                //取消一个正在执行的任务,onCancelled方法将会被调用  
                mTask.cancel(true);  
            }  
        });  
        progressBar = (ProgressBar) findViewById(R.id.progress_bar);  
        textView = (TextView) findViewById(R.id.text_view);  
  
    }  
  
    private class MyTask extends AsyncTask<String, Integer, String> {  
        //onPreExecute方法用于在执行后台任务前做一些UI操作  
        @Override  
        protected void onPreExecute() {  
            Log.i(TAG, "onPreExecute() called");  
            textView.setText("loading...");  
        }  
  
        //doInBackground方法内部执行后台任务,不可在此方法内修改UI  
        @Override  
        protected String doInBackground(String... params) {  
            Log.i(TAG, "doInBackground(Params... params) called");  
            try {  
                HttpClient client = new DefaultHttpClient();  
                HttpGet get = new HttpGet(params[0]);  
                HttpResponse response = client.execute(get);  
                if (response.getStatusLine().getStatusCode() == HttpStatus.SC_OK) {  
                    HttpEntity entity = response.getEntity();  
                    InputStream is = entity.getContent();  
                    long total = entity.getContentLength();  
                    ByteArrayOutputStream baos = new ByteArrayOutputStream();  
                    byte[] buf = new byte[1024];  
                    int count = 0;  
                    int length = -1;  
                    while ((length = is.read(buf)) != -1) {  
                        baos.write(buf, 0, length);  
                        count += length;  
                        //调用publishProgress公布进度,最后onProgressUpdate方法将被执行  
                        publishProgress((int) ((count / (float) total) * 100));  
                        //为了演示进度,休眠500毫秒  
                        Thread.sleep(500);  
                    }  
                    return new String(baos.toByteArray(), "gb2312");  
                }  
            } catch (Exception e) {  
                Log.e(TAG, e.getMessage());  
            }  
            return null;  
        }  
  
        //onProgressUpdate方法用于更新进度信息  
        @Override  
        protected void onProgressUpdate(Integer... progresses) {  
            Log.i(TAG, "onProgressUpdate(Progress... progresses) called");  
            progressBar.setProgress(progresses[0]);  
            textView.setText("loading..." + progresses[0] + "%");  
        }  
  
        //onPostExecute方法用于在执行完后台任务后更新UI,显示结果  
        @Override  
        protected void onPostExecute(String result) {  
            Log.i(TAG, "onPostExecute(Result result) called");  
            textView.setText(result);  
  
            execute.setEnabled(true);  
            cancel.setEnabled(false);  
        }  
  
        //onCancelled方法用于在取消执行中的任务时更改UI  
        @Override  
        protected void onCancelled() {  
            Log.i(TAG, "onCancelled() called");  
            textView.setText("cancelled");  
            progressBar.setProgress(0);  
  
            execute.setEnabled(true);  
            cancel.setEnabled(false);  
        }  
    }  
}  
```

``` xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="fill_parent"  
    android:layout_height="fill_parent">  
    <Button  
        android:id="@+id/execute"  
        android:layout_width="fill_parent"  
        android:layout_height="wrap_content"  
        android:text="execute"/>  
    <Button  
        android:id="@+id/cancel"  
        android:layout_width="fill_parent"  
        android:layout_height="wrap_content"  
        android:enabled="false"  
        android:text="cancel"/>  
    <ProgressBar   
        android:id="@+id/progress_bar"   
        android:layout_width="fill_parent"   
        android:layout_height="wrap_content"   
        android:progress="0"  
        android:max="100"  
        style="?android:attr/progressBarStyleHorizontal"/>  
    <ScrollView  
        android:layout_width="fill_parent"   
        android:layout_height="wrap_content">  
        <TextView  
            android:id="@+id/text_view"  
            android:layout_width="fill_parent"   
            android:layout_height="wrap_content"/>  
    </ScrollView>  
</LinearLayout>  
```

### 参考

1. 详解Android中AsyncTask的使用 https://blog.csdn.net/liuhe688/article/details/6532519/
