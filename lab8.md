# 实验8：Android设备编程

##8.1 知识点
###1.   
添加权限
###2.  
设置布局
###3
获取GPS信息
##8.2 实例讲解
###1. 获取设备当前位置信息
简要说明<br>
获取手机的GPS信息,显示获取信息中的经纬度信息。当位置变化时，会重新获取手机的PGS信息，显示最新获取到的经纬度信息。

详细步骤<br>
1修改AndroidManifest.xml，添加权限
```
  <uses-permission Android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>  
   <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />  
```
<br>
2判断GPS是否正常启动
```
if(!lm.isProviderEnabled(LocationManager.GPS_PROVIDER)){
            Toast.makeText(this, "请开启GPS导航...", Toast.LENGTH_SHORT).show();
            //返回开启GPS导航设置界面
            Intent intent = new Intent(Settings.ACTION_LOCATION_SOURCE_SETTINGS);
            startActivityForResult(intent,0);
            return;
        }
```
<br>
3设置查询条件
```
        String bestProvider = lm.getBestProvider(getCriteria(), true);
        //获取位置信息
        //如果不设置查询要求，getLastKnownLocation方法传人的参数为LocationManager.GPS_PROVIDER
        Location location= lm.getLastKnownLocation(bestProvider);
        updateView(location);
        //监听状态
        lm.addGpsStatusListener(listener);
        //绑定监听，有4个参数
        //参数1，设备：有GPS_PROVIDER和NETWORK_PROVIDER两种
        //参数2，位置信息更新周期，单位毫秒
        //参数3，位置变化最小距离：当位置距离变化超过此值时，将更新位置信息
        //参数4，监听
        //备注：参数2和3，如果参数3不为0，则以参数3为准；参数3为0，则通过时间来定时更新；两者为0，则随时刷新

        // 1秒更新一次，或最小位移变化超过1米更新一次；
        //更新位置
        lm.requestLocationUpdates(LocationManager.GPS_PROVIDER, 5000, 10, locationListener);
    }
```
<br>
4位置监听
```
private LocationListener locationListener=new LocationListener() {

             public void onLocationChanged(Location location) {
            updateView(location);

                 ContentValues cv = new ContentValues();
                 cv.put("longitude",String.valueOf(location.getLongitude()));
                 cv.put("latitude", String.valueOf(location.getLatitude()));
                 dbWrite.insert("whereYou", null, cv);
                 refresh();//刷新数据库
```
<br>
5GPS开启/关闭时触发
```
 public void onProviderEnabled(String provider) {
            Location location=lm.getLastKnownLocation(provider);
            updateView(location);
        }
        
  public void onProviderDisabled(String provider) {
            updateView(null);
        }

```
<br>
6状态监听
```
GpsStatus.Listener listener = new GpsStatus.Listener() {
        public void onGpsStatusChanged(int event) {
            switch (event) {
                //第一次定位
                case GpsStatus.GPS_EVENT_FIRST_FIX:
                    Log.i(TAG, "第一次定位");
                    break;
                //卫星状态改变
                case GpsStatus.GPS_EVENT_SATELLITE_STATUS:
                    Log.i(TAG, "卫星状态改变");
                    //获取当前状态
                    GpsStatus gpsStatus=lm.getGpsStatus(null);
                    //获取卫星颗数的默认最大值
                    int maxSatellites = gpsStatus.getMaxSatellites();
                    //创建一个迭代器保存所有卫星
                    Iterator<GpsSatellite> iters = gpsStatus.getSatellites().iterator();
                    int count = 0;
                    while (iters.hasNext() && count <= maxSatellites) {
                        GpsSatellite s = iters.next();
                        count++;
                    }
                    System.out.println("搜索到："+count+"颗卫星");
                    break;
                //定位启动
                case GpsStatus.GPS_EVENT_STARTED:
                    Log.i(TAG, "定位启动");
                    break;
                //定位结束
                case GpsStatus.GPS_EVENT_STOPPED:
                    Log.i(TAG, "定位结束");
                    break;
            }
        };
    };
```
<br>
7更新要显示的文本信息
```
 private void updateView(Location location){
        if(location!=null){
            editText.setText("位置信息\n\n经度：");
            editText.append(String.valueOf(location.getLongitude()));
            editText.append("\n纬度：");
            editText.append(String.valueOf(location.getLatitude()));
            editText.append("\n海拔：");
            editText.append(String.valueOf(location.getAltitude()));



            editText.append("\n时间：");
            editText.append(str);
        }else{
            //清空EditText对象
            editText.getEditableText().clear();
        }
    }
``` 

###2. 播放MP3音乐
简要说明……  
详细步骤……

###3. 相机 @WL101ZYF
简要说明
利用此程序，可以调用手机内原有的照相机或者有摄像功能的程序，并以此实现照相功能，最后保存到SD卡或其他存储相片的位置中。
详细步骤
1、建立一个项目。
2、在配置文件中写入下面三条语句：
```
<!--android中使用摄像机的权限  -->
<uses-permission android:name="android.permission.CAMERA" />
<!--android中创建于删除文件的权限  -->
<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
<!--android中写入SDCARD的权限  -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```
3、在XML文件正确合适的位置里加入以下代码：
```
<SurfaceView
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/surfaceview"/>

    <RelativeLayout
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:visibility="gone"
        android:id="@+id/buttonlayout">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentRight="true"
            android:layout_alignParentBottom="true"
            android:layout_marginRight="5dp"
            android:text="@string/takepicture"
            android:onClick="takepicture"
            android:id="@+id/takepicture" />



        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_toLeftOf="@id/takepicture"
            android:layout_alignTop="@id/takepicture"
            android:layout_marginRight="20dp"
            android:text="@string/autofocus"
            android:onClick="takepicture"
            android:id="@+id/autofocus" />
```
4、打开java文件，写入如下代码
```
    private View layout;
    private Camera camera;


    @Override
    protected void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        //设置窗口没有标题
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        //设置窗口全屏
        getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
                WindowManager.LayoutParams.FLAG_FULLSCREEN);
        setContentView(R.layout.activity_net1314080903102);


        //利用layout方法，找到两个按钮控件
        layout = this.findViewById(R.id.buttonlayout);
        //获取摄像头窗口
        SurfaceView surfaceView = (SurfaceView) this.findViewById(R.id.surfaceview);
        //将获取的摄像头填满整个窗口
        surfaceView.getHolder().setType(SurfaceHolder.SURFACE_TYPE_PUSH_BUFFERS);
        //设置窗口分辨率
        surfaceView.getHolder().setFixedSize(176, 144);
        //保持屏幕高亮，不要锁机
        surfaceView.getHolder().setKeepScreenOn(true);
        //设置摄像头被调用监听事件
        surfaceView.getHolder().addCallback(new SurfaceCallback());


    }

    /*
     * 通过switch (v.getId()) 选择拍照事件和对焦事件
     */
    public void takepicture(View v) {
        if (camera != null) {
            switch (v.getId()) {
                case R.id.takepicture:
                    //拍照片经过压缩处理后的图片调用MyPictureCallback方法
                    camera.takePicture(null, null, new MyPictureCallback());
                    break;
                case R.id.autofocus:
                    //如果不想得到对焦事件，传送NULL事件进去
                    camera.autoFocus(null);

                default:
                    break;
            }


        }

    }
      /*
     * 获取图片对象
     * */

    private final class MyPictureCallback implements PictureCallback {
        public void onPictureTaken(byte[] data, Camera camera) {

            try {
                //将文件存储在SD卡的dcim目录，并以系统时间将文件命名
                File jpgFile = new File("/sdcard/dcim/",
                        java.lang.System.currentTimeMillis() + ".jpg");
                //文件输出流对象
                FileOutputStream outStream = new FileOutputStream(jpgFile);
                //将文件数据存储到文件中
                outStream.write(data);
                //关闭输出流
                outStream.close();
                //开始预览照片NN
                camera.startPreview();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

    /*
     * 设置摄像头参数
     */
    private final class SurfaceCallback implements android.view.SurfaceHolder.Callback {

        public void surfaceCreated(SurfaceHolder holder) {
            try {
                //打开摄像头
                camera = Camera.open();
                //获取摄像头参数对象
                Camera.Parameters parameters = camera.getParameters();
                //设置摄像头分辨率
                parameters.setPreviewSize(800, 480);
                //设置摄像头捕获画面的频率为每秒5个画面
                parameters.setPreviewFrameRate(5);
                //设置拍摄照片的大小
                parameters.setPictureSize(1024, 768);
                //设置捕捉图像的JPEG画质
                parameters.setJpegQuality(80);
                //把参数返回给摄像头
                camera.setParameters(parameters);
                //显示摄像头捕获画面
                camera.setPreviewDisplay(holder);
                //开始预览摄像头
                camera.startPreview();
                //获取摄像头详细参数，并且打印出来
                //Log.i("MainActivity", parameters.flatten());

            } catch (Exception e) {
                e.printStackTrace();
            }

        }

        public void surfaceChanged(SurfaceHolder holder, int format, int width, int heigh) {

        }

        public void surfaceDestroyed(SurfaceHolder holder) {
            //如果摄像头不使用时，关闭摄像头
            if (camera != null) {
                camera.release();
                camera = null;
            }


        }

    }


    /*
     *屏幕被触摸事件
     *屏幕被按下后，显示相对布局里面的两个按钮
     */
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (event.getAction() == MotionEvent.ACTION_DOWN) {
            layout.setVisibility(ViewGroup.VISIBLE);

        }

        return super.onTouchEvent(event);

    }
}
```

###4. 执行操作系统命令安装Apk

项目做了一个秒装软件的功能，其实就是静默安装啦。所谓的静默安装，就是不用弹出系统的安装界面，在不影响用户任何操作的情况下不知不觉地将程序装好。
秒装其实是需要ROOT权限的静默安装。静默安装的原理很简单，就是调用Android系统的pm install命令就可以了，但是pm命令系统是不授予我们权限调用的，因此只能在拥有ROOT权限的手机上去申请权限才行。
首先新建一个项目，然后创建一个Net1314080903112SilentInstall类作为静默安装功能的实现类，代码如下所示：
``` 
package edu.hzuapps.androidworks.homeworks.net1314080903112;

import android.util.Log;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.charset.Charset;



public class Net1314080903112SilentInstall {

    /**
     * 执行具体的静默安装逻辑，需要手机ROOT。
     * @param apkPath
     *          要安装的apk文件的路径
     * @return 安装成功返回true，安装失败返回false。
     */
    public boolean install(String apkPath) {
        boolean result = false;
        DataOutputStream dataOutputStream = null;
        BufferedReader errorStream = null;
        try {
            // 申请su权限
            Process process = Runtime.getRuntime().exec("su");
            dataOutputStream = new DataOutputStream(process.getOutputStream());
            // 执行pm install命令
            String command = "pm install -r " + apkPath + "\n";
            dataOutputStream.write(command.getBytes(Charset.forName("utf-8")));
            dataOutputStream.flush();
            dataOutputStream.writeBytes("exit\n");
            dataOutputStream.flush();
            process.waitFor();
            errorStream = new BufferedReader(new InputStreamReader(process.getErrorStream()));
            String msg = "";
            String line;
            // 读取命令的执行结果
            while ((line = errorStream.readLine()) != null) {
                msg += line;
            }
            Log.d("TAG", "install msg is " + msg);
            // 如果执行结果中包含Failure字样就认为是安装失败，否则就认为安装成功
            if (!msg.contains("Failure")) {
                result = true;
            }
        } catch (Exception e) {
            Log.e("TAG", e.getMessage(), e);
        } finally {
            try {
                if (dataOutputStream != null) {
                    dataOutputStream.close();
                }
                if (errorStream != null) {
                    errorStream.close();
                }
            } catch (IOException e) {
                Log.e("TAG", e.getMessage(), e);
            }
        }
        return result;
    }

}
``` 
可以看到，Net1314080903112SilentInstall类中只有一个install()方法，所有静默安装的逻辑都在这个方法中了，那么我们具体来看一下这个方法。首先调用了Runtime.getRuntime().exec("su")方法，在这里先申请ROOT权限，不然的话后面的操作都将失败。然后开始组装静默安装命令，命令的格式就是pm install -r <apk路径>，-r参数表示如果要安装的apk已经存在了就覆盖安装的意思，apk路径是作为方法参数传入的。接下来的几行就是执行上述命令的过程，注意安装这个过程是同步的，因此我们在下面调用了process.waitFor()方法，即安装要多久，我们就要在这里等多久。等待结束之后说明安装过程结束了，接下来我们要去读取安装的结果并进行解析，解析的逻辑也很简单，如果安装结果中包含Failure字样就说明安装失败，反之则说明安装成功。
接下来搭建调用这个方法的环境，修改net1314080903112activity_main.xml中的代码，以及新建net1314080903112activity_file_explorer.xml和net1314080903112list_item.xml作为文件选择器的布局文件。
然后新建Net1314080903112FileExplorerActivity作为文件选择器的Activity，接着修改Net1314080903112MainActivity中的代码，如下所示：

```
package edu.hzuapps.androidworks.homeworks.net1314080903112;

import android.content.Intent;
import android.net.Uri;
import android.provider.Settings;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import java.io.File;


public class Net1314080903112MainActivity extends AppCompatActivity {

    TextView apkPathText;

    String apkPath;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.net1314080903112activity_main);
        apkPathText = (TextView) findViewById(R.id.apkPathText);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == 0 && resultCode == RESULT_OK) {
            apkPath = data.getStringExtra("apk_path");
            apkPathText.setText(apkPath);
        }
    }

    public void onChooseApkFile(View view) {
        Intent intent = new Intent(this, Net1314080903112FileExplorerActivity.class);
        startActivityForResult(intent, 0);
    }

    public void onSilentInstall(View view) {
        if (!isRoot()) {
            Toast.makeText(this, "没有ROOT权限，不能使用秒装", Toast.LENGTH_SHORT).show();
            return;
        }
        if (TextUtils.isEmpty(apkPath)) {
            Toast.makeText(this, "请选择安装包！", Toast.LENGTH_SHORT).show();
            return;
        }
        final Button button = (Button) view;
        button.setText("安装中");
        new Thread(new Runnable() {
            @Override
            public void run() {
                Net1314080903112SilentInstall installHelper = new Net1314080903112SilentInstall();
                final boolean result = installHelper.install(apkPath);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        if (result) {
                            Toast.makeText(Net1314080903112MainActivity.this, "安装成功！", Toast.LENGTH_SHORT).show();
                        } else {
                            Toast.makeText(Net1314080903112MainActivity.this, "安装失败！", Toast.LENGTH_SHORT).show();
                        }
                        button.setText("秒装");
                    }
                });

            }
        }).start();

    }

    public void onForwardToAccessibility(View view) {
        Intent intent = new Intent(Settings.ACTION_ACCESSIBILITY_SETTINGS);
        startActivity(intent);
    }

    public void onSmartInstall(View view) {
        if (TextUtils.isEmpty(apkPath)) {
            Toast.makeText(this, "请选择安装包！", Toast.LENGTH_SHORT).show();
            return;
        }
        Uri uri = Uri.fromFile(new File(apkPath));
        Intent localIntent = new Intent(Intent.ACTION_VIEW);
        localIntent.setDataAndType(uri, "application/vnd.android.package-archive");
        startActivity(localIntent);
    }

    /**
     * 判断手机是否拥有Root权限。
     * @return 有root权限返回true，否则返回false。
     */
    public boolean isRoot() {
        boolean bool = false;
        try {
            if (Runtime.getRuntime().exec("su").getOutputStream() == null) {
                return false;
            } else {
                return true;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return bool;
    }

}
```
可以看到，在Net1314080903112MainActivity中，我们对四个按钮点击事件的回调方法都进行了定义，当点击“选择安装包”按钮时就会调用onChooseApkFile()方法，当点击“秒装”按钮时就会调用onSilentInstall()方法。在onChooseApkFile()方法方法中，我们通过Intent打开了Net1314080903112FileExplorerActivity，然后在onActivityResult()方法当中读取选择的apk文件路径。在onSilentInstall()方法当中，先判断设备是否ROOT，如果没有ROOT就直接return，然后判断安装包是否已选择，如果没有也直接return。接下来我们开启了一个线程来调用Net1314080903112SilentInstall.install()方法，因为安装过程会比较耗时，如果不开线程的话主线程就会被卡住，不管安装成功还是失败，最后都会使用Toast来进行提示。
最后在配置一下AndroidManifest.xml文件即可。
```

