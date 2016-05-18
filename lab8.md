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

###3. 相机 
简要说明……  
详细步骤…… Markdown

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
