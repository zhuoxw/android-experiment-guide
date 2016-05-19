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
简要说明：海贼王连连看游戏主界面是由Net1314080903126main.xml 文件通过调用Net1314080903126CtrlView.java  Net1314080903126GameView.java
Net1314080903126OnePieceGame.java 来显示主界面的以及游戏规则和游戏玩法的等。
详细步骤：
1.   游戏的主界面是通过Net1314080903126main.xml 来显示的，代码：
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:orientation="vertical" android:layout_width="match_parent"
	android:layout_height="wrap_content">
	<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
	 	android:layout_width="fill_parent"	android:layout_height="wrap_content">
	 	<TableRow>
			<ProgressBar android:id="@+id/pb" android:layout_width="fill_parent"
				android:layout_height="wrap_content" style="?android:attr/progressBarStyleHorizontal" 
				android:layout_weight="9"/>
			<TextView android:layout_height="wrap_content" android:layout_width="wrap_content"
				android:text="@string/remain_time" android:layout_weight="1"/>
			<TextView android:layout_height="wrap_content" android:layout_width="wrap_content"
				android:id="@+id/show_remainTime" android:layout_weight="1"/>	
		</TableRow>
	</TableLayout>
	
	<edu.hzuapps.androidworks.homeworks.net1314080903126.Net1314080903126CtrlView
		android:id="@+id/cv"
		android:layout_width="wrap_content" android:layout_height="fill_parent" />
</LinearLayout>

其中：<edu.hzuapps.androidworks.homeworks.net1314080903126.Net1314080903126CtrlView
		android:id="@+id/cv"
		android:layout_width="wrap_content" android:layout_height="fill_parent" />
		这是显示海贼王人物的以及人物界面布局。
然后：<TableRow>
			<ProgressBar android:id="@+id/pb" android:layout_width="fill_parent"
				android:layout_height="wrap_content" style="?android:attr/progressBarStyleHorizontal" 
				android:layout_weight="9"/>
			<TextView android:layout_height="wrap_content" android:layout_width="wrap_content"
				android:text="@string/remain_time" android:layout_weight="1"/>
			<TextView android:layout_height="wrap_content" android:layout_width="wrap_content"
				android:id="@+id/show_remainTime" android:layout_weight="1"/>	
		</TableRow>
		这是显示时间进度条的，以及时间 剩余时间（秒）和倒计时300.
		这是海贼王连连看的界面的大致介绍。
2.  下面详细解释一下海贼王连连看。
（1）.  public class Net1314080903126GameView extends View {

	public final int row = 10;
	public final int col = 10;
	public float width;
	public float height;
	private int selY;
	private int selX;
	public boolean isLine = false;
	public int grid[][] = new int[row][col];
	private Rect selRect = new Rect();
	public int lineType = 0;
	public final int V_LINE = 1;
	public final int H_LINE = 1;
	public final int ONE_C_LINE = 2;
	public final int TWO_C_LINE = 3;
	public int much = 0;
	Point[] p;
	public int[] imageType = new int[] { R.drawable.net1314080903126aa, R.drawable.net1314080903126bb,
			R.drawable.net1314080903126cc, R.drawable.net1314080903126dd, R.drawable.net1314080903126ee, R.drawable.net1314080903126ff,
			R.drawable.net1314080903126gg, R.drawable.net1314080903126hh, R.drawable.net1314080903126ii, R.drawable.net1314080903126jj,
			R.drawable.net1314080903126kk, R.drawable.net1314080903126ll, R.drawable.net1314080903126mm, R.drawable.net1314080903126nn,
			R.drawable.net1314080903126oo, R.drawable.net1314080903126pp};
	public Bitmap[] image;
	public List<Integer> type = new ArrayList<Integer>();
	这个是Net1314080903126GameView.java文件中，调用图片。public final int row = 10;
	public final int col = 10;图片的布局分布十行十列显示。以及显示大小尺寸。调用drawable文件下的图片显示在界面上。
	
(2). <TextView android:layout_height="wrap_content" android:layout_width="wrap_content"
				android:text="@string/remain_time" android:layout_weight="1"/>
	这是显示剩余时间（秒）
(3). <ProgressBar android:id="@+id/pb" android:layout_width="fill_parent"
				android:layout_height="wrap_content" style="?android:attr/progressBarStyleHorizontal" 
				android:layout_weight="9"/>
	这是显示倒计时时间进度条的。
(4). <TextView android:layout_height="wrap_content" android:layout_width="wrap_content"
				android:id="@+id/show_remainTime" android:layout_weight="1"/>	
        这是显示300时间的跑秒。
(5).public boolean onCreateOptionsMenu(Menu menu) {
		// TODO Auto-generated method stub
		menu.add(0, START_ID, 0, R.string.newgame);
		menu.add(0, REARRARY_ID, 0, R.string.rearrage);
		menu.add(0, END_ID, 0, R.string.exit);
		return super.onCreateOptionsMenu(menu);
	}
	这是页面上方选项菜单，可以选择（新游戏，重排列以及退出游戏）这三个功能。
(6).public AlertDialog dialogForSucceed() {
		AlertDialog.Builder builder = new AlertDialog.Builder(this);
		builder.setIcon(R.drawable.icon).setMessage(R.string.succeedInfo)
				.setPositiveButton(R.string.next,
						new DialogInterface.OnClickListener() {
							@Override
							public void onClick(DialogInterface dialog,
									int which) {
								// TODO Auto-generated method stub
								dormant = dormant - 300;
								newPlay();
							}
						}).setNeutralButton(R.string.again_challenge,
						new DialogInterface.OnClickListener() {
							@Override
							public void onClick(DialogInterface dialog,
									int which) {
								// TODO Auto-generated method stub
								newPlay();
							}
						});
		return builder.create();
	}

	public AlertDialog dialogForFail() {
		AlertDialog.Builder builder = new AlertDialog.Builder(this);
		builder.setIcon(R.drawable.icon).setMessage(R.string.failInfo)
				.setPositiveButton(R.string.again_challenge,
						new DialogInterface.OnClickListener() {
							@Override
							public void onClick(DialogInterface dialog,
									int which) {
								// TODO Auto-generated method stub
								newPlay();
							}
						}).setNegativeButton(R.string.exit,
						new DialogInterface.OnClickListener() {
							@Override
							public void onClick(DialogInterface dialog,
									int which) {
								// TODO Auto-generated method stub
								isCancel=false;
								finish();
							}
						});
		return builder.create();
	}
	这是显示300秒内完成游戏提示通往下一关，以及没有完成游戏提示你不气馁。
以上就是海贼王练练看看游戏界面的详细实现过程。
	

###4. 制作扫雷游戏界面
简要说明……  
详细步骤……  
1. 
```  

```  
源代码：!https://github.com/hzuapps/android-labs/tree/master/app/src/main/java/edu/hzuapps/androidworks/homeworks/com1314080901110
