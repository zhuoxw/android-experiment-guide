# 实验8：Android设备编程

## 8.1 基于位置的服务简介
说到只有在移动设备上才能实现的技术， 很容易就让人联想到基于位置的服务 (LocationBased Service)。由于移动设备相比于电脑可以随身携带，我们通过地理定位的技术就可以随时得知自己所在的位置，从而围绕这一点开发出很多有意思的应用。

## 8.2 实例讲解
### 8.2.1 获取设备当前位置信息
其实，归根结底，基于位置的服务所围绕的核心就是要确定出自己所在的位置，这在
Android 中并不困难， 主要借助 LocationManager 这个类就可以实现了。
下面我们首先学习一下 LocationManager 的基本用法，然后再通过一个例子来尝试获取一下自己当前的位置。
（PS：这里所写的代码建议你都在手机上运行，DDMS 虽然也提供了在模拟器中模拟地理位置的功能，但在手机上得到真实的位置数据，你的感受会更加深刻。）

#### 8.2.1.1 LocationManager 的基本用法
毫无疑问， 要想使用LocationManager就必须要先获取到它的实例， 我们可以调用Context的 getSystemService()方法获取到。 getSystemService()方法接收一个字符串参数用于确定获取系统的哪个服务， 这里传入 Context.LOCATION_SERVICE 即可。 因此， 获取 LocationManager的实例就可以写成：`
LocationManager locationManager = (LocationManager)
getSystemService(Context.LOCATION_SERVICE);`
接着我们需要选择一个位置提供器来确定设备当前的位置。Android 中一般有三种位置
提供器可供选择，GPS_PROVIDER、NETWORK_PROVIDER 和 PASSIVE_PROVIDER。其中前两种使用的比较多，分别表示使用 GPS 定位和使用网络定位。这两种定位方式各有特点，GPS定位的精准度比较高，但是非常耗电，而网络定位的精准度稍差，但耗电量比较少。我们应该根据自己的实际情况来选择使用哪一种位置提供器， 当位置精度要求非常高的时候，最好使用 GPS_PROVIDER，而一般情况下，使用 NETWORK_PROVIDER会更加得划算。
（PS：需要注意的是，定位功能必须要由用户主动去启用才行，不然任何应用程序都无法获取到手机当前的位置信息。这里你只需要进入系统APP权限控制菜单中允许程序启用定位权限即可。）

接着我们将选择好的位置提供器传入到 getLastKnownLocation()方法中， 就可以得到一个 Location对象，如下所示：`
String provider = LocationManager.NETWORK_PROVIDER;
Location location = locationManager.getLastKnownLocation(provider);`
这个 Location 对象中包含了经度、纬度、海拔等一系列的位置信息，然后从中取出我们所关心的那部分数据即可。

如果有些时候你想让定位的精度尽量高一些，但又不确定 GPS 定位的功能是否已经启
用，这个时候就可以先判断一下有哪些位置提供器可用，如下所示：`
List<String> providerList = locationManager.getProviders(true);`
可以看到，getProviders()方法接收一个布尔型参数，传入 true就表示只有启用的位置提供器才会被返回。之后再从 providerList 中判断是否包含 GPS 定位的功能就行了。

另外，调用 getLastKnownLocation()方法虽然可以获取到设备当前的位置信息，但是用户是完全有可能带着设备随时移动的， 那么我们怎样才能在设备位置发生改变的时候获取到最新的位置信息呢？
不用担心，LocationManager 还提供了一个 requestLocationUpdates()方法，只要传入一个 LocationListener 的实例，并简单配置几个参数就可以实现上述功能了，
写法如下：`
locationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 5000, 10,new LocationListener() {
Override
public void onStatusChanged(String provider, int status, Bundle
extras) {
}
Override
public void onProviderEnabled(String provider) {
}
Override
public void onProviderDisabled(String provider) {
}
@Override
public void onLocationChanged(Location location) {
}
});`
这里 requestLocationUpdates()方法接收四个参数，第一个参数是位置提供器的类型；第二个参数是监听位置变化的时间间隔，以毫秒为单位；第三个参数是监听位置变化的距离间隔，以米为单位；第四个参数则是 LocationListener监听器。这样的话LocationManager每隔5秒钟会检测一下位置的变化情况，当移动距离超过 10米的时候，就会调用 LocationListener的 onLocationChanged()方法，并把新的位置信息作为参数传入。
好了，关于 LocationManager 的用法基本就是这么多，下面我们就通过一个例子来尝试一下吧。

#### 8.2.1.2 确定自己位置的经纬度
通过上一小节的学习，你会发现 LocationManager 的用法并不复杂，那么本小节中我们来编写一个可以获取当前位置经纬度信息的程序吧。
新建一个 LocationTest项目，修改 activity_main.xml 中的代码，如下所示：`
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent" >
<TextView
android:id="@+id/position_text_view"
android:layout_width="wrap_content"
android:layout_height="wrap_content" />
</LinearLayout>`
布局文件中的内容实在是太简单了，只有一个 TextView 控件，用于稍后显示设备位置
的经纬度信息。
然后修改 MainActivity 中的代码，如下所示：`
public class MainActivity extends Activity {
private TextView positionTextView;
private LocationManager locationManager;
private String provider;
@Override
protected void onCreate(Bundle savedInstanceState) throws SecurityException {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);
positionTextView = (TextView) findViewById(R.id.position_text_view);
locationManager = (LocationManager) getSystemService(Context.
LOCATION_SERVICE);
// 获取所有可用的位置提供器
List<String> providerList = locationManager.getProviders(true);
if (providerList.contains(LocationManager.GPS_PROVIDER)) {
provider = LocationManager.GPS_PROVIDER;
} else if (providerList.contains(LocationManager.NETWORK_PROVIDER)) {
provider = LocationManager.NETWORK_PROVIDER;
} else {
// 当没有可用的位置提供器时，弹出Toast提示用户
Toast.makeText(this, "No location provider to use",
Toast.LENGTH_SHORT).show();
return;
}
Location location = locationManager.getLastKnownLocation(provider);
if (location != null) {
// 显示当前设备的位置信息
showLocation(location);
}
locationManager.requestLocationUpdates(provider, 5000, 1,
locationListener);
}
protected void onDestroy() {
super.onDestroy();
try {
 // 关闭程序时将监听器移除
 locationManager.removeUpdates(locationListener);
 } catch (SecurityException e) {
e.printStackTrace();
}
}
LocationListener locationListener = new LocationListener() {
@Override
public void onStatusChanged(String provider, int status, Bundle
extras) {
}
@Override
public void onProviderEnabled(String provider) {
}
@Override
public void onProviderDisabled(String provider) {
}
@Override
public void onLocationChanged(Location location) {
// 更新当前设备的位置信息
showLocation(location);
}
};
private void showLocation(Location location) {
String currentPosition = "latitude is " + location.getLatitude() + "\n" + "longitude is " + location.getLongitude();
positionTextView.setText(currentPosition);
}
}`
这里并没有什么复杂的逻辑，基本全是我们在上一小节中学到的知识。
在 onCreate()方法中首先是获取到了 LocationManager 的实例，然后调用 getProviders()方法去得到所有可用的位置提供器，接下来再调用 getLastKnownLocation()方法就可以获取到记录当前位置信息的 Location 对象了，这里我们将 Location 对象传入到 showLocation()方法中，经度和纬度的值就会显示到 TextView 上了。然后为了要能监测到位置信息的变化，下面又调用了requestLocationUpdates()方法来添加一个位置监听器，设置时间间隔是 5 秒，距离间隔是 1米，并在 onLocationChanged()方法中时时更新 TextView 上显示的经纬度信息。最后当程序关闭时，我们还需要调用 removeUpdates()方法来将位置监听器移除，以保证不会继续耗费手机的电量。

另外，获取设备当前的位置信息也是要声明权限的，因此还需要修改 AndroidManifest.xml
中的代码，如下所示：`
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
package="com.example.locationtest"
android:versionCode="1"
android:versionName="1.0" >
……
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
……
</manifest>`
现在运行一下程序，就可以看到手机当前位置的经纬度信息了，如图 8.1 所示。
![Alt text](./1463559122953.png)

之后如果你拿着手机随处移动， 就可以看到界面上的经纬度信息是会变化的。 由此证实，我们的程序确实已经在正常工作了。

### 8.2.2 反向地理编码，看得懂的位置信息
话说回来，刚才我们虽然成功获取到了设备当前位置的经纬度信息，但遗憾的是，这种经纬值一般人是根本看不懂的，相信谁也无法立刻答出南纬 25 度、东经 148 度是什么地方吧？为了能够更加直观地阅读位置信息， 本节中我们就来学习一下， 如何通过反向地理编码，将经纬值转换成看得懂的位置信息。
#### 8.2.2.1 Geocoding API 的用法
其实 Android 本身就提供了地理编码的 API，主要是使用 GeoCoder 这个类来实现的。
它可以非常简单地完成正向和反向的地理编码功能， 从而轻松地将一个经纬值转换成看得懂的位置信息。
不过，非常遗憾的是，GeoCoder长期存在着一些较为严重的 bug，在反向地理编码的时候会有一定的概率不能解析出位置的信息，这样就无法保证位置解析的稳定性，因此我们不得不去寻找 GeoCoder的替代方案。
还算比较幸运，谷歌又提供了一套 Geocoding API，使用它的话也可以完成反向地理编码的工作，只不过它的用法稍微复杂了一些，但稳定性要比 GeoCoder强得多。
本小节中我们只是学习一下 GeocodingAPI的简单用法， 更详细的用法请参考官方文档： https://developers.google.com/maps/documentation/geocoding/
GeocodingAPI 的工作原理并不神秘，其实就是利用了我们上一章中学习的 HTTP 协议。
在手机端我们可以向谷歌的服务器发起一条 HTTP 请求， 并将经纬度的值作为参数一同传递过去，然后服务器会帮我们将这个经纬值转换成看得懂的位置信息，再将这些信息返回给手机端，最后手机端去解析服务器返回的信息，并进行处理就可以了。
GeocodingAPI 中规定了很多接口，其中反向地理编码的接口如下：
http://maps.googleapis.com/maps/api/geocode/json?latlng=40.714224,-73.961452&sensor=true_or_false
我们来仔细看下这个接口的定义， 其中http://maps.googleapis.com/maps/api/geocode/是固定的，表示接口的连接地址。json 表示希望服务器能够返回 JSON 格式的数据，这里也可以指定成 xml。 
latlng=40.714224,-73.96145 表示传递给服务器去解码的经纬值是北纬 40.714224度， 西经 73.96145度。 
sensor=true_or_false 表示这条请求是否来自于某个设备的位置传感器，通常指定成 false 即可。
如果发送 http://maps.googleapis.com/maps/api/geocode/jsonlatlng=40.714224,-73.96145&sensor=false 这样一条请求给服务器，我们将会得到一段非常长的 JSON 格式的数据，其中会包括如下部分内容：
"formatted_address" : "277 Bedford Avenue, 布鲁克林纽约州 11211美国"
从这段内容中我们就可以看出北纬 40.714224度， 西经 73.96145 度对应的地理位置是在哪里了。如果你想查看服务器返回的完整数据，在浏览器中访问上面的网址即可。这样的话，使用 Geocoding API 进行反向地理编码的工作原理你就已经搞清楚了，那么难点其实就在于如何从服务器返回的数据中通过解析JSON数据获得我们想要的那部分信息了。
#### 8.2.2.2 对经纬度进行解析
使用 Geocoding API 进行反向地理编码的流程相信你已经很清楚了，我们先要发送一个HTTP 请求给谷歌的服务器，然后再对返回的 JSON 数据进行解析。发送 HTTP 请求的方式我们准备使用 HttpClient，解析 JSON 数据的方式使用 JSONObject。
修改 MainActivity 中的代码，如下所示：`
public class MainActivity extends Activity {
public static final int SHOW_LOCATION = 0;
……
private void showLocation(final Location location) { 
new Thread(new Runnable() { 
@Override 
public void run() { 
URL urlObject = null; 
HttpURLConnection urlConnection = null; 
InputStream in = null; 
try { 
// 组装反向地理编码的接口地址 
StringBuilder url = new StringBuilder(); 
url.append("http://maps.google.com/maps/api/geocode/json?latlng="); 
url.append(location.getLatitude()).append(","); 
url.append(location.getLongitude()); 
// 指定语言，保证服务器会返回中文数据 
url.append("&language=zh-CN&sensor=true"); 
urlObject = new URL(url.toString()); 
urlConnection = (HttpURLConnection) urlObject.openConnection(); 
urlConnection.setRequestMethod("GET"); 
urlConnection.setRequestProperty("Content-Type", "application/json; charset=UTF-8"); 
urlConnection.connect(); 
// 判断请求码是否200，否则为失败 
if (urlConnection.getResponseCode() == 200) { 
in = urlConnection.getInputStream(); // 获取输入流 
BufferedReader reader = new BufferedReader(new 
InputStreamReader(in)); 
StringBuilder response = new StringBuilder(); 
String line; 
while ((line = reader.readLine()) != null) { 
response.append(line); 
} 
JSONObject jsonObject = new JSONObject(response.toString()); 
// 获取result节点下的位置信息 
JSONArray resultArray = jsonObject.getJSONArray("results"); 
if (resultArray.length() > 0) { 
JSONObject subObject = resultArray.getJSONObject(0); 
// 取出格式化后的位置信息 
String address = subObject.getString("formatted_address"); 
Message message = new Message(); 
message.what = SHOW_LOCATION; 
message.obj = address; 
handler.sendMessage(message); 
} 
} else { 
Log.d("MainActivity", "xyz " + "请求url失败！"); } 
} catch (Exception e) { 
e.printStackTrace();} 
} 
}).start(); 
} 
private Handler handler = new Handler() {
public void handleMessage(Message msg) {
switch (msg.what) {
case SHOW_LOCATION:
String currentPosition = (String) msg.obj;
positionTextView.setText(currentPosition);
break;
default:
break;
}
}
};
}`
观察 showLocation()方法，由于我们要在这里发起网络请求，因此必须开启一个子线程。在子线程中首先是通过 StringBuilder 组装了一个反向地理编码接口地址的字符串， 然后使用HttpURLConnection 去请求这个地址就好了。 
注意，在url设置中要将语言类型指定为简体中文，不然服务器会默认返回英文的位置信息。接下来就是对服务器返回的 JSON 数据进行解析了。由于一个经纬度的值有可能包含了好几条街道，因此服务器通常会返回一组位置信息，这些信息都是存放在 results 结点下的。
在得到了这些位置信息后只需要取其中的第一条就可以了， 通常这也是最接近我们位置的那一条。之后就可以从 formatted_address 结点中取出格式化后的位置信息了，这种位置信息你就完全可以看得懂了。
不过别忘了，目前我们还是在子线程当中的，因此在这里无法直接将得到的位置信息显示到 TextView 上。所有这里需要使用异步消息处理机制，才能成功地将返回来的位置信息显示到界面上。
由于这里我们使用到了网络功能，因此还需要在 AndroidManifest.xml 中添加权限声明，如下所示：`
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
package="com.example.locationtest"
android:versionCode="1"
android:versionName="1.0" >
……
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.INTERNET" />
……
</manifest>`
好了，现在可以重新运行一下程序了，结果如图 8.2 所示。
![Alt text](./1463559155247.png)

可以看到，手机当前的位置信息已经成功显示出来了！如果你带着手机移动了较远的距离，界面上显示的位置也会跟着一起变化的。
当然，在这个例子中我们只是对服务器返回的 JSON 数据进行了最简单的解析，位置信息是作为整体取出的，其实你还可以进行更精确的解析，将国家名、城市名、街道名、甚至邮政编码等作为独立的信息取出，更加有趣的功能就等着你自己去进行研究了。

### 8.2.2 使用第三方的库实现反向地理编码
做到这里，你对整个定位功能应该也了解的差不多了。但是你可能会觉得奇怪，明明按照上面的代码完成了一个程序，但最后却始终没办法显示当前所在的位置。细心的你通过经过代码调试会发现，从接口返回的数据一直是为null，这时候你应该会在想，是不是代码本身就是错的，这作者不会是在坑我吧。
其实，这样说就真的是冤枉我了，代码本身是没有错的，但是因为是调用了谷歌的接口，所以才没办法获取到返回数据，这个中原因，你应该也明白了吧。
所以，下面要介绍的就是通过国内的一些地图sdk提供商来实现定位当前位置的功能，这类提供商也比较多，比如百度，高德或者腾讯等，这里主要介绍如何用高德SDK来实现自动定位，因为比起其他来说，这个API用起来会更加简单方便。当然你要使用其他也是可以的，这类SDK的使用方法都大同小异，看完下面介绍之后，相信你使用其他第三方SDK也可以游刃有余。

#### 8.2.2.1 高德SDK开发环境配置
1、注册开发者，创建应用
这个几乎是所有开放平台都通用的做法，无外乎注册帐号，成为开发者，然后创建一个Android应用，会为你分配一个key绑定你的服务。

2、下载SDK
从网站下载并解压得到定位包“AMap_Location_V2.x.x.jar“。

3、在Android Studio上进行配置
打开Android Studio编译器，切换到project查看方式，如图所示：
![Alt text](./1463642865384.png)

将下载的定位SDK的jar包复制到libs目录下，如果有老版本定位jar包在其中，请删除。如图所示：
![Alt text](./1463642886374.png)

4、配置AndroidMainfest.xml文件
首先，请在application标签中声明service组件,每个app拥有自己单独的定位service。
`<service android:name="com.amap.api.location.APSService"></service>`
接下来声明使用权限`
<!--用于进行网络定位-->
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"></uses-permission>
<!--用于访问GPS定位-->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"></uses-permission>
<!--获取运营商信息，用于支持提供运营商信息相关的接口-->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>
<!--用于访问wifi网络信息，wifi信息会用于进行网络定位-->
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>
<!--这个权限用于获取wifi的获取权限，wifi信息会用来进行网络定位-->
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE"></uses-permission>
<!--用于访问网络，网络定位需要上网-->
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
<!--用于读取手机当前的状态-->
<uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>
<!--写入扩展存储，向扩展卡写入数据，用于写入缓存定位数据-->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>`
最后设置Key，在application标签中加入
`<meta-data android:name="com.amap.api.v2.apikey" android:value="key">//开发者申请的key</meta-data>`
在value后面填入你设定应用时所得到的key，至此，整个高德sdk的配置工作就基本完成了，接下来我们来演示一下如何使用高德SDK来实现定位功能。

#### 8.2.2.2 使用高德SDK实现自动定位
在演示开始之前，我们首先要对高德SDK有一定的了解。
高德定位服务包含GPS和网络定位（Wi-Fi和基站定位）两种能力。定位SDK将GPS、网络定位能力进行了封装，以三种定位模式对外开放，他们具体的特性如下：
高精度定位模式：会同时使用网络定位和GPS定位，优先返回最高精度的定位结果；
低功耗定位模式：不会使用GPS，只会使用网络定位（Wi-Fi和基站定位）；
仅用设备定位模式：不需要连接网络，只使用GPS进行定位，这种模式下不支持室内环境的定位。

知道了这些之后，我们就来真正的实现自动定位功能。
第一，我们要初始化定位客户端，设置监听。
（PS：请在主线程中声明AMapLocationClient类对象，需要传Context类型的参数。推荐用getApplicationConext()方法获取全进程有效的context。）`
//声明AMapLocationClient类对象
public AMapLocationClient mLocationClient = null;
//声明定位回调监听器
public AMapLocationListener mLocationListener = new AMapLocationListener();
//初始化定位
mLocationClient = new AMapLocationClient(getApplicationContext());
//设置定位回调监听
mLocationClient.setLocationListener(mLocationListener);`

第二，我们要配置定位参数，在Activity中的onCreate()中行初始化即可启动定位。
设置定位参数包括：定位模式（高精度定位模式，低功耗定位模式和仅设备定位模式），是否返回地址信息等。`
//声明mLocationOption对象
public AMapLocationClientOption mLocationOption = null;
//初始化定位参数
mLocationOption = new AMapLocationClientOption();
//设置定位模式为高精度模式，Battery_Saving为低功耗模式，Device_Sensors是仅设备模式
mLocationOption.setLocationMode(AMapLocationMode.Hight_Accuracy);
//设置是否返回地址信息（默认返回地址信息）
mLocationOption.setNeedAddress(true);
//设置是否只定位一次,默认为false
mLocationOption.setOnceLocation(false);
//设置是否强制刷新WIFI，默认为强制刷新
mLocationOption.setWifiActiveScan(true);
//设置是否允许模拟位置,默认为false，不允许模拟位置
mLocationOption.setMockEnable(false);
//设置定位间隔,单位毫秒,默认为2000ms
mLocationOption.setInterval(2000);
//给定位客户端对象设置定位参数
mlocationClient.setLocationOption(mLocationOption);
//启动定位
mlocationClient.startLocation();`

第三，我们要实现AMapLocationListener接口，获取定位结果。
AMapLocationListener接口只有onLocationChanged方法可以实现，用于接收异步返回的定位结果，参数是AMapLocation类型。`
public void onLocationChanged(AMapLocation amapLocation) {
    if (amapLocation != null) {
        if (amapLocation.getErrorCode() == 0) {
        //定位成功回调信息，设置相关消息
        amapLocation.getLocationType();//获取当前定位结果来源，如网络定位结果，详见定位类型表
        amapLocation.getLatitude();//获取纬度
        amapLocation.getLongitude();//获取经度
        amapLocation.getAccuracy();//获取精度信息
        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = new Date(amapLocation.getTime());
        df.format(date);//定位时间
        amapLocation.getAddress();//地址，如果option中设置isNeedAddress为false，则没有此结果，网络定位结果中会有地址信息，GPS定位不返回地址信息。
        amapLocation.getCountry();//国家信息
        amapLocation.getProvince();//省信息
        amapLocation.getCity();//城市信息
        amapLocation.getDistrict();//城区信息
        amapLocation.getStreet();//街道信息
                amapLocation.getStreetNum();//街道门牌号信息
        amapLocation.getCityCode();//城市编码
        amapLocation.getAdCode();//地区编码
                amapLocation.getAOIName();//获取当前定位点的AOI信息
    } else {
              //显示错误信息ErrCode是错误码，errInfo是错误信息，详见错误码表。
        Log.e("AmapError","location Error, ErrCode:"
            + amapLocation.getErrorCode() + ", errInfo:"
            + amapLocation.getErrorInfo());
        }
    }
}`

最后，我们要停止自动定位。
停止定位：`
mlocationClient.stopLocation();//停止定位`
销毁定位客户端：
销毁定位客户端之后，若要重新开启定位请重新New一个AMapLocationClient对象。`
mlocationClient.onDestroy();//销毁定位客户端。`

看到这里，相信你对于如何运用高德SDK来实现自动定位也已经有了一定的了解了。因为代码整体上跟上一节的差不多，所有这里就不再重复演示代码了。只要做好这几步，便可以在页面上显示出自己当前所在的位置，这肯定也是难不倒你了吧。
其实高德sdk的使用远远不止于这些方面，还有很多其他的功能等待你去发掘，比如可以在APP直观的用地图显示出你当前所在的位置等，这些东西在官方文档上面都有详细的介绍，感兴趣的你可以去研究一下，祝你早日完成自己想要的APP啦 (●'◡'●)
