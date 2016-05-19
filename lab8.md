# 实验8：Android设备编程

##8.1 知识点
###1.   

###2.  

##8.2 实例讲解
###1. 获取设备当前位置信息
简要说明……  
详细步骤……  

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
``` 
代码
``` 


