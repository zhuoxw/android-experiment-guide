# 实验7：Android网络编程

## 7.1 知识点

###1.     

###2.     

## 7.2 实例步骤

###1. 根据位置信息从中国天气网获取天气信息
简要说明……  
详细步骤……  

###2. Socket编程
简要说明  
　　这个例子只用了TCP协议下的Socket编程，因此这里只讨论TCP的情况。  
　　在java中，服务器端socket、bind、listen等操作被封装在ServerSocket类库中，客户端socket、connect等操作被封装在Socket类库中。如需了解这些操作细节，可学习C语言下的socket编程。  
详细步骤      
服务器端步骤（单线程）    
1). servs = new ServerSocket(port);     //port 为指定的端口号  
2). socket = servs.accept();            //等待连接，程序会阻塞在这里  
3). 读写socket。  

客户端步骤   
1). socket = Socket("ip_str", port);    //ip_str 为服务器端IP，port为端口号，和服务器端指定的要一样    
2). 读写socket。                        //1).中已经执行连接服务器的操作，所以接下来可以直接读写。  
注意：    
1). 客户端连接服务器的操作要放在新线程里面。  
2). 需要添加权限：<uses-permission android:name="android.permission.INTERNET"/>  
3). 真机调试的时候，真机和主机要在同一个局域网。  
核心代码  
服务器端：  

客户端代码：
