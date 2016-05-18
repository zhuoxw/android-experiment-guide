# 实验4：Android界面设计

##8.1 知识点
###1.   

###2.  

##8.2 实例讲解
###1. 设计仿微信导航界面
简要说明……  
详细步骤……  

###2. 制作黑白棋游戏界面
简要说明:我做的是一个黑白棋的应用，在主界面是用垂直布局来弄的，中间会涉及到几个相对布局和表格布局，还是相对来说比较简单的。
详细步骤：
1.  android:background="@drawable/net13140803138background" //一开始用了这一句调用了drawable里的背景图片作为背景。
2.  android:orientation="vertical"//然后这里是说明我是垂直方向进行线性布局。
3.  然后我就开始按照布局放控件了，其中：
<ImageView
        android:layout_width="match_parent"
        android:layout_weight="1"
        android:layout_height="0dp"
        android:scaleType="fitXY"
        android:id="@+id/top1"
        android:src="@drawable/net13140803138top"/>   
这一部分代码即我最上面的一个ImageView，调用了drawable/net13140803138top图片。
4.  
  <LinearLayout
        android:layout_width="match_parent"
        android:layout_marginTop="10dp"
        android:orientation="horizontal"
        android:layout_height="0dp"
        android:layout_weight="3">

        <ImageView
            android:layout_width="0dp"
            android:layout_weight="2"
            android:layout_height="match_parent"
            android:scaleType="fitCenter"
            android:src="@drawable/net13140803138daojian"
            />


    </LinearLayout>
这里我是用了一个水平方向的线性布局，这里值得一提的是，我放了3个位置，但是我只放了一个控件，调用了drawable/net13140803138daojian图片，如果我后面增加了新的功能，可以有位置添加进去。
5.  这里则是由12个黑白棋子组成的一个布局，由12个黑白棋图片组成，代码会有点长，但是代码其实是比较简单的
<RelativeLayout
        android:layout_width="match_parent"
        android:layout_weight="7"
        android:layout_height="0dp">

        <GridLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:rowCount="3"
            android:columnCount="4"
            android:layout_centerInParent="true"
            >

            <ImageView
                android:id="@+id/iv1"
                android:layout_width="@dimen/width_of_chess1"  android:layout_margin="@dimen/chess_margin1"
                android:layout_height="@dimen/width_of_chess1"
                android:scaleType="fitCenter"
                android:src="@drawable/net13140803138white_chess"/>

            <ImageView
                android:id="@+id/iv2"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138black_chess"
                />

            <ImageView
                android:id="@+id/iv3"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138black_chess"
                />

            <ImageView
                android:id="@+id/iv4"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:src="@drawable/net13140803138white_chess"
                android:layout_height="@dimen/width_of_chess1" />

            <ImageView
                android:id="@+id/iv5"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138black_chess"/>

            <ImageView
                android:id="@+id/iv6"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138white_chess"
                />

            <ImageView
                android:id="@+id/iv7"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138white_chess"
                />

            <ImageView
                android:id="@+id/iv8"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:src="@drawable/net13140803138black_chess"
                android:layout_height="@dimen/width_of_chess1" />

            <ImageView
                android:id="@+id/iv9"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138black_chess"/>

            <ImageView
                android:id="@+id/iv10"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138white_chess"
                />

            <ImageView
                android:id="@+id/iv11"
                android:scaleType="fitCenter"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:layout_height="@dimen/width_of_chess1"
                android:src="@drawable/net13140803138black_chess"
                />

            <ImageView
                android:id="@+id/iv12"
                android:layout_width="@dimen/width_of_chess1" android:layout_margin="@dimen/chess_margin1"
                android:scaleType="fitCenter"
                android:src="@drawable/net13140803138white_chess"
                android:layout_height="@dimen/width_of_chess1" />


        </GridLayout>

    </RelativeLayout>

这一段的代码我是用了一个相对布局，里面在用一个表格布局放在这个相对布局的中间，调用了12个ImageView放在表格布局里面，其中，调用了drawable/net13140803138white_chess和drawable/net13140803138black_chess两个黑白棋子的图片。
6.  这里我要加一个声音的开关放在右下角，所以要加一个相对布局
   <RelativeLayout
        android:layout_width="match_parent"
        android:orientation="horizontal"
        android:layout_height="wrap_content">

        <CheckBox
            android:id="@+id/soundSwitch"
            android:layout_alignParentRight="true"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Sound"
            android:textColor="#FF000000"/>

    </RelativeLayout>
所以这里我用了一个相对布局，然后放一个CheckBox在这相对布局里面。
7.  <ImageButton
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:contentDescription="@string/app_name"
        android:id="@+id/startButton"
        android:src="@drawable/net13140803138start"
        android:scaleType="fitXY"
        android:background="#00000000"
        android:layout_marginBottom="6dp"/>
这里也是调用了一个ImageButton作为一个开始按钮，其中调用了drawable/net13140803138start图片。
至此，我的主界面就完成了。
```

```
###3. 制作练练看游戏界面
简要说明……  
详细步骤……  

###4. 制作扫雷游戏界面
简要说明……  
详细步骤……  
1. 
```  

```  
源代码：!https://github.com/hzuapps/android-labs/tree/master/app/src/main/java/edu/hzuapps/androidworks/homeworks/com1314080901110
