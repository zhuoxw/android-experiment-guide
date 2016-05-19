# 实验8：Android设备编程

##8.1 知识点
###1.   

###2.  

##8.2 实例讲解
###1. 获取设备当前位置信息
简要说明……  
详细步骤……  

###2. 播放MP3音乐
####1.简要说明  
#####1.实现方式
      使用Android系统本身设备
      播放本地SD卡内的MP3音乐
#####2.实现前提
	  Android系统是一个基于Linux平台的开源移动操作系统，换句话说，Android是一种类Linux系统。Linux系统核心部分的权限，自然也被Android系统所继承。简单而言，对于一个普通文件权限属性分为读、写、执行，Android系统中不同开发人员创建的不同文件，文件所有权都被收归系统，而系统的授权分配归类等待授权请求。同时Android系统也发展出了一些与Linux系统不相同的权限，因为该系统的每一个APP理论上都是独立隔离的，APP之间的访问是有限制的（权限）。系统提供的资源都是采取“不请求不启动”的原则，保证体积较小的手机系统运行的速度足够。
######1.具有对SD卡内容的读写权限
	 在开发过程中，开发人员常常会使用自身项目包内的文件，由于这些文件都是开发人员自行导入生成，所有这些文件的所有者默认便是开发者，开发者对所有的项目包具有完全读写权限，调用修改乃至删除统统符合所有者权限。而当开发人员需要使用到系统其它不属于自己的资源时，则必须申请权限。
######2.service声明权限
	 service的创建应该是具有限制的，保证不会出现不需要这项service而它偏偏被系统分配空间且长时间占用，系统却认为这种占用是合法的，不予以处理的矛盾情况
#####3.系统API
	Android系统默认提供了媒体播放的MediaPlayer类，其中有以下API可以使用：
```
setDataSource (String path) ;//设置播放文件的地址
setOnSeekCompleteListener(OnSeekCompleteListener);//设置读取完整后的监听器
 start();//开始播放
 stop();//停止播放
```
#####4.注意事项		
	 在Android 程序开发中，activity里面都不能具有耗时太长的执行语句，因为这会造成图形界面的长时间未响应，让使用者觉得该程序卡死了，于是在Android系统本身便有限制，一旦某个Activity 耗时太长，该程序将会被强制停止
	在AndroidManifest.xml的不同地方声明不同部分的权限。
####2.详细步骤
#####1.请求SD卡读写权限
```
 <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> 
  <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" /> 
```
#####2.service声明权限
```
<service android:name=".Net1314080903219MediaPlayerService" android:enabled="true" android:exported="true" /> 
```
#####3.播放音频视频的权限
```
<uses-permission android:name="android.permission.MEDIA_CONTENT_CONTROL" /> 
  <uses-permission android:name="android.permission.BIND_VOICE_INTERACTION" /> 
  <uses-permission android:name="android.permission.CAPTURE_SECURE_VIDEO_OUTPUT" /> 
  <uses-permission android:name="android.permission.CAPTURE_VIDEO_OUTPUT" /> 
```
#####4.使用MediaPlayer完成设计
```
MediaPlayer player = new MediaPlayer();

    public Net1314080903219MediaPlayerService() {

      /*  try {
            player.reset();
            player.setDataSource(path);
            player.prepare();
        } catch (IOException e) {
            e.printStackTrace();
        }*/
    }


    @Override
    public IBinder onBind(Intent arg0) {
        // TODO Auto-generated method stub
        return null;
    }

    //在这里我们需要实例化MediaPlayer对象
    public void onCreate(){

        super.onCreate();
        //我们从raw文件夹中获取一个应用自带的mp3文件

        System.out.println("sfsfsfsf  dsf fdfd fsdf sf ffsfs");


    }

    /**
     * 该方法在SDK2.0才开始有的，替代原来的onStart方法
     */
    public int onStartCommand(Intent intent, int flags, int startId){
        if(!player.isPlaying()){
        //    System.out.println(String.valueOf(intent.getCharSequenceArrayExtra("song")));
            try {
                player.reset();
                player.setDataSource(intent.getStringExtra("song"));
                player.prepare();
                player.start();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return START_STICKY;
    }

    public void onDestroy(){
        //super.onDestroy();
        if(player.isPlaying()){
            player.stop();
        }
        player.release();
    }




    //后退播放进度
    public void haveFun(){
        if(player.isPlaying() && player.getCurrentPosition()>2500){
            player.seekTo(player.getCurrentPosition()-2500);
        }
    }
```
#####5.运行效果图










