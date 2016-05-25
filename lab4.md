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
简要说明：这里介绍的是扫雷游戏的初始界面和游戏进行界面的实现方法，它是通过获取每个格子的id值来调用显示相应的背景图片。
详细步骤： 
1. 先获取每个格子对象，再给每个格子对象编号或下标获取id值，代码如下：
```  
public int getCount() {
        return level*level;
    }
    /**
     * 方法：获取每个格子对象
     * @param position 格子编号，位置下标
     * @return 格子类型的GameGroundEntity
     * */
    @Override
    public GridEntity getItem(int position) {
//        调用GameGroundEntity中的getEntity方法获取格子对象
        return gameGround.getEntity(position);
    }
    /**
     * 方法：通过适配器给每个格子对象编号或下标获取id值
     * @return long类型，在java中，byte和short可自动转换为int，int可自动转换为long
     * */
    @Override
    public long getItemId(int position) {
        return position;
    }


```  
2.设置格子对象的背景图片， 不同状态下设置不同的背景图片（i00,i13等是图片的名字）
``` 
@param grid :格子对象
     * */
    public int getRes(GridEntity grid){
//        设置格子对象的背景图片的ID为0
        int resID=0;
//        判断，如果格子对象被标记了且标记正确
        if(grid.isFlag()&&!grid.isFlagWrong()){
            resID=R.drawable.i_flag;
        }
//        判断，如果格子对象被标记了但标记不正确
        else if(grid.isFlag()&&grid.isFlagWrong()){
            resID=R.drawable.i14;
        }
//        判断，如果格子对象没有被点击，isShow()属性为false
        else if(!grid.isShow()){
            resID=R.drawable.i00;
        }
//        判断，格子对象是地雷且非自动显示
        else if(grid.isBoom()&&!grid.isAutoShow()){
            resID=R.drawable.i13;
        }
//        判断，格子对象是地雷，自动显示
        else if(grid.isBoom()&&grid.isAutoShow()){
            resID=R.drawable.i12;
        }
//        判断，格子周围没有地雷，是空白格
        else if(grid.getBoomsCount()==0){
            resID=R.drawable.i09;
        }
//        判断，格子中卫有地雷，个数为1-8个
        else if(grid.getBoomsCount()!=0){
//            动态拼接图片名，格式为图片名称，图片类型，资源所在包名
            resID=context.getResources().getIdentifier("i0"+grid.getBoomsCount(),"drawable",context.getPackageName());
        }
        return resID;
    }
``` 
源代码：!https://github.com/hzuapps/android-labs/tree/master/app/src/main/java/edu/hzuapps/androidworks/homeworks/com1314080901110


###5. 制作围住神经猫游戏界面 
简要说明：简单版的围住神经猫游戏的背景是一排排的圆圈，可以用二维数组创建，而这些圆圈有三种状态，分别为猫可以走并可以设置路障的位置（灰色），猫所处的位置（红色），猫不能走并已开启路障的位置（橘色），这些状态定义在Dot类中，显然其应该有坐标X和Y；游戏的背景等我都放在Playground类中，背景是基于SurfaceView开发的，而里面还有很多功能，如猫和已设路障的初始化位置，猫如何躲路障，如何判断路可走，判断处在边界位置等等；最后在Activity中调用Playground。 

详细步骤： 
1. 建立Dot类，用来记录每个场景中的元素它的X,Y坐标点的状态。并不会直接参与界面的响应和界面的绘制。每一个点都是一个抽象的对象，需要把每一个点抽象为一个类，然后让每一个圆圈继承于这个类，或者直接把它实现为这个类的实例。每个点有三个状态：灰色-猫可走的路径；橘色-路障的状态
无法改变；红色-猫当前的位置。

```
public class Net1314080903142Dot {
	
	int x,y;//当前点的X，Y坐标
	int status;//记录这个点的状态
	//三个表征圆点状态静态常量
	public static final int STATUS_ON = 1;//已经开启路障的状态
	public static final int STATUS_OFF = 0;//代表灰色可走路径
	public static final int STATUS_IN = 9;//猫当前的位置
    //三个数字不同即可，具体用哪个数字无所谓

	
       //指定X，Y坐标
	public Net1314080903142Dot(int x, int y) {
		super();
		this.x = x;
		this.y = y;
		status = STATUS_OFF;
	}

       //指定geter和sette方法
	public int getX() {
		return x;
	}

	public void setX(int x) {
		this.x = x;
	}

	public int getY() {
		return y;
	}

	public void setY(int y) {
		this.y = y;
	}

	public int getStatus() {
		return status;
	}

	public void setStatus(int status) {
		this.status = status;
	}

	//同时设置X，Y坐标的方法
	public void setXY(int x,int y) {
		this.y = y;
		this.x = x;
	}	
}
```

2.建立Playground类，用来绘制界面还有实现游戏的各种算法。以下只详细介绍界面的绘制还有每个圆圈状态转化的过程。

```
public class Net1314080903142Playground  extends SurfaceView implements OnTouchListener{
    //界面的响应和界面的绘制在SurfaceView完成，触摸事件的响应通过OnTouchListener接口实现
		
	
	private static  int WIDTH = 40;
	private static final int ROW = 10;//行高：每行储存10个元素
	private static final int COL = 10;//列宽：每列储存10个元素
	private static final int BLOCKS = 15;//默认添加的路障数量
	
	
	private Dot Net1314080903142matrix[][];//声明二维数组来存放点元素
	private Dot Net1314080903142cat;//声明猫这个点

	public Net1314080903142Playground(Context context) {
		super(context);//使用Context创建当前类构造函数
		getHolder().addCallback(callback);//将Callback对象指定给getholder
		matrix = new Net1314080903142Dot[ROW][COL];//将行高，列宽传递进去，指定数组大小
		for (int i = 0; i < ROW; i++) {//循环添加数据
			for (int j = 0; j < COL; j++) {
				matrix[i][j] = new Net1314080903142Dot(j, i);/*X，Y坐标值和行列值是相反的。
				即通过查找列值获得X坐标，查找行值获得Y坐标*/
			}
		}
		setOnTouchListener(this);//设定为自己的触摸监听器
		initGame();//调用游戏初始化
	}


	//坐标反转：封装一个getDot函数实现X，Y坐标反过来传递，所有的操作通过X，Y调用
	private Net1314080903142Dot getDot(int x,int y) {
		return matrix[y][x];
	}


       //实现猫移动到下一个点
       private void MoveTo(Net1314080903142Dot one) {
		one.setStatus(Net1314080903142Dot.STATUS_IN);//one的状态设置为猫所处的点
		getDot(cat.getX(), cat.getY()).setStatus(Net1314080903142Dot.STATUS_OFF);;//将猫当前点的状态复位
		cat.setXY(one.getX(), one.getY());//将猫移动到新的点
	}
	//猫的移动
	private void move() {
		if (isAtEdge(cat)) {
			lose();return;/猫处于游戏边缘，失败
		}
		Vector<Net1314080903142Dot> avaliable = new Vector<Net1314080903142Dot>();//avaliable容器记录可用点
		Vector<Net1314080903142Dot> positive = new Vector<Net1314080903142Dot>();//positive容器记录这个方向上可以直接到达屏幕边缘的路径
		HashMap<Net1314080903142Dot, Integer> al = new HashMap<Net1314080903142Dot, Integer>();//al容器记录方向
		for (int i = 1; i < 7; i++) {//如果当前猫被6个邻点围住
			Net1314080903142Dot n = getNeighbour(cat, i);
			if (n.getStatus() == Net1314080903142Dot.STATUS_OFF) {
				avaliable.add(n);//如果相邻点可用，把它添加到avaliable记录器中
				al.put(n, i);//为al传入方向i
				if (getDistance(n, i) > 0) {
					positive.add(n);//当它有一个路径可以直接到达屏幕边缘，把n传递进positive中
					
				}
			}
		}
                //移动算法的优化
		if (avaliable.size() == 0) {
			win();//周围的6个点都不可走，没有可用点，成功围住猫
		}else if (avaliable.size() == 1) {
			MoveTo(avaliable.get(0));//只有一个方向可走，可用点有一个，移动到这个可用点上
		}else{//有多个方向可走
			Net1314080903142Dot best = null;
			if (positive.size() != 0 ) {//存在可以直接到达屏幕边缘的走向
				System.out.println("向前进");
				int min = 999;//999远大于场景中的所有可用步长，其他数也可
				for (int i = 0; i < positive.size(); i++) {
					int a = getDistance(positive.get(i), al.get(positive.get(i)));
					if (a < min) {
						min = a;//把最短路径长度传给min
						best = positive.get(i);//选出拥有最短路径的点
					}
				}
				MoveTo(best);
			}else {//所有方向都存在路障
				System.out.println("躲路障");
				int max = 0;
				for (int i = 0; i < avaliable.size(); i++) {
					int k = getDistance(avaliable.get(i), al.get(avaliable.get(i)));
					if (k <= max) {//所有方向都存在路障，距离k为负数
						max = k;
						best = avaliable.get(i);//选出拥有最短路径的点
					}
				}
				MoveTo(best);//移动到最短路径的下一点
			}
		}
	}



	//实现界面绘制，在redraw方法中将所有元素以图形化显示出来，也就是将它绘制在Canvas对象上
        private void redraw() {
		Canvas c = getHolder().lockCanvas();//锁定画布
		c.drawColor(Color.LTGRAY);//设置颜色为浅灰色
		Paint paint = new Paint();//创建Paint对象
		paint.setFlags(Paint.ANTI_ALIAS_FLAG);//开启抗锯齿，优化视频质量
		
                //用两个For循环嵌套将所有的点显示到界面中来
		for (int i = 0; i < ROW; i++) {
			int offset = 0;//引入偏移量
			if (i%2 != 0) {
				offset = WIDTH/2;//对偶数行进行缩进
			}
			for (int j = 0; j < COL; j++) {
				Net1314080903142Dot one = getDot(j, i);//将坐标赋值给内部变量one
                                //由于每个点对应的三种状态颜色不一样，要用一个switch语句
				switch (one.getStatus()) {
				case Net1314080903142Dot.STATUS_OFF:
					paint.setColor(0xFFEEEEEE);//STATUS_OFF状态时设置颜色为浅灰色
					break;
				case Net1314080903142Dot.STATUS_ON:
					paint.setColor(0xFFFFAA00);//STATUS_ON状态时设置颜色为橘色
					break;
				case Net1314080903142Dot.STATUS_IN:
					paint.setColor(0xFFFF0000);//STATUS_IN状态时设置颜色为红色
					break;

				default:
					break;
				}
				c.drawOval(new RectF(one.getX()*WIDTH+offset, one.getY()*WIDTH, 
						(one.getX()+1)*WIDTH+offset, (one.getY()+1)*WIDTH), paint);
	                             /*在Canvas画布上画椭圆并界定它的上下左右边界宽度且有错位*/
                           }
			
		}
		getHolder().unlockCanvasAndPost(c);//取消Canvas的锁定，吧绘图内容更新到界面上
	}

	//为Surfaceview添加Callback
	Callback callback = new Callback() {//声明并实例化一个Callback接口
		
		@Override
		public void surfaceDestroyed(SurfaceHolder arg0) {
			// TODO Auto-generated method stub
			
		}
		
		@Override
		public void surfaceCreated(SurfaceHolder arg0) {
			// TODO Auto-generated method stub
			redraw();//执行redraw函数，在界面第一次显示时将指定的内容显示到界面上
		}
		
		@Override
                 //使用surfaceChanged方法来适配不同的屏幕尺寸
		public void surfaceChanged(SurfaceHolder arg0, int arg1, int arg2, int arg3) {
                        
                        //surfacechanged方法包含四个参数：SurfaceHolder holder，int format，int width，int height
			// TODO Auto-generated method stub
			WIDTH = arg2/(COL+1);//需要修改width，即arg2。
			redraw();//重绘界面
		}
	};
	//游戏初始化：分别对可走路径位置，猫的位置和路障位置进行初始化
	private void initGame() {
                //用for循环将所有点设置为STATUS_OFF，即可用状态
		for (int i = 0; i < ROW; i++) {
			for (int j = 0; j < COL; j++) {
				matrix[i][j].setStatus(Net1314080903142Dot.STATUS_OFF);
			}
		}
		cat = new Net1314080903142Dot(4, 5);//设置猫的起始点
		getDot(4, 5).setStatus(Net1314080903142Dot.STATUS_IN);//把猫的起始点的状态设置为STATUS_IN，才能记录猫的位置

                //用for循环随机的指定15个点的坐标作为路障
		for (int i = 0; i < BLOCKS;) {
			int x = (int) ((Math.random()*1000)%COL);
			int y = (int) ((Math.random()*1000)%ROW);//随机获取1对坐标点
			if (getDot(x, y).getStatus() == Net1314080903142Dot.STATUS_OFF) {//对当前可用路径点进行选择
				getDot(x, y).setStatus(Net1314080903142Dot.STATUS_ON);//并把这个点设置为路障
				i++;//循环内自加避免当前路障被重复添加
				//System.out.println("Block:"+i);
			}
		}
	}

	@Override
        //触摸事件的处理
	public boolean onTouch(View arg0, MotionEvent e) {
		if (e.getAction() == MotionEvent.ACTION_UP) {/当用户触摸之后手离开屏幕释放的瞬间才对事件进行响应
             //	Toast.makeText(getContext(), e.getX()+":"+e.getY(), Toast.LENGTH_SHORT).show();
                        //将屏幕的坐标转换为游戏的坐标
			int x,y;
			y = (int) (e.getY()/WIDTH);//横向状态下，奇、偶数行有坐标偏移，而纵向的Y值是不变的，将y进行转换
			if (y%2 == 0) {
				x = (int) (e.getX()/WIDTH);//奇数行直接将屏幕的X坐标转换成游戏的X坐标
			}else {
				x = (int) ((e.getX()-WIDTH/2)/WIDTH);//偶数行偏移半个元素宽度，故需减去WIDTH/2
			}
                         //数组越界异常时，对坐标进行保护
			if (x+1 > COL || y+1 > ROW) {
				initGame();//触摸超出边界时初始化游戏
			}else if(getDot(x, y).getStatus() == Net1314080903142Dot.STATUS_OFF){
				getDot(x, y).setStatus(Net1314080903142Dot.STATUS_ON);//当这个点可用时被点击之后设定为路障状态
				move();
			}
			redraw();//将改变更新到界面
		}
		return true;
	}
}
```

3.最后创建Activity,调用Playground。

```
public class Net1314080903142Activity extends Activity {

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		setContentView(new Net1314080903142Playground(this));//Context把MainActivity的this传递进去
	}

}
```

备注：实现该游戏的其他算法未列出，有兴趣的可以在该网站看全部代码：https://github.com/hzuapps/android-labs/issues/139

###6. 自动滚动Banner图片


