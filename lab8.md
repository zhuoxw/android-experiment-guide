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
