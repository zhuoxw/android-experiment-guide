# 实验9：Android综合实验

###1. 播报数字

+1. 声明并初始化SoundPool类、HashMap<String,Integer>类的实例化对象sp和spMap
 ```  
private HashMap<String, Integer> spMap=null;    //用于管理音频流
private SoundPool sp;   // 音频池
private int soundId;    // 音频ID
protected void onCreate(Bundle savedInstanceState) {
  .
  .
  .
  // 初始化HashMap<String, Integer>类的实例对象spMap
  spMap = new HashMap<>();
  // 初始化SoundPool类的实例对象sp，并设置最多可容纳16个音频流
  sp = new SoundPool(16, AudioManager.STREAM_MUSIC, 0);
  .
  .
  .
}
```  
2. 加载音频文件，并用HashMap类来管理加载的音频文件；为按钮添加点击事件
```  
        // 用SoundPool类的load方法加载指定音频文件，并用soundId保存返回的音频ID。
        // 用HashMap类来管理这些音频流
        soundId = sp.load(this, R.raw.zero, 1);
        spMap.put("0", soundId);
        soundId = sp.load(this, R.raw.one, 1);
        spMap.put("1", soundId);
        soundId = sp.load(this, R.raw.two, 1);
        spMap.put("2", soundId);
        soundId = sp.load(this, R.raw.three, 1);
        spMap.put("3", soundId);
        soundId = sp.load(this, R.raw.four, 1);
        spMap.put("4", soundId);
        soundId = sp.load(this, R.raw.five, 1);
        spMap.put("5", soundId);
        soundId = sp.load(this, R.raw.six, 1);
        spMap.put("6", soundId);
        soundId = sp.load(this, R.raw.seven, 1);
        spMap.put("7", soundId);
        soundId = sp.load(this, R.raw.eight, 1);
        spMap.put("8", soundId);
        soundId = sp.load(this, R.raw.nine, 1);
        spMap.put("9", soundId);
        soundId = sp.load(this, R.raw.ac, 1);
        spMap.put("ac", soundId);
        soundId = sp.load(this, R.raw.del, 1);
        spMap.put("del", soundId);
        soundId = sp.load(this, R.raw.div, 1);
        spMap.put("div", soundId);
        soundId = sp.load(this, R.raw.dot, 1);
        spMap.put(".", soundId);
        soundId = sp.load(this, R.raw.equal, 1);
        spMap.put("equal", soundId);
        soundId = sp.load(this, R.raw.minus, 1);
        spMap.put("minus", soundId);
        soundId = sp.load(this, R.raw.mul, 1);
        spMap.put("mul", soundId);
        soundId = sp.load(this, R.raw.plus, 1);
        spMap.put("plus", soundId);
        // 为按钮设置点击监听事件
        findViewById(R.id.btn0).setOnClickListener(this);
        findViewById(R.id.btn1).setOnClickListener(this);
        findViewById(R.id.btn2).setOnClickListener(this);
        findViewById(R.id.btn3).setOnClickListener(this);
        findViewById(R.id.btn4).setOnClickListener(this);
        findViewById(R.id.btn5).setOnClickListener(this);
        findViewById(R.id.btn6).setOnClickListener(this);
        findViewById(R.id.btn7).setOnClickListener(this);
        findViewById(R.id.btn8).setOnClickListener(this);
        findViewById(R.id.btn9).setOnClickListener(this);
        findViewById(R.id.btnadd).setOnClickListener(this);
        findViewById(R.id.btnsub).setOnClickListener(this);
        findViewById(R.id.btnmul).setOnClickListener(this);
        findViewById(R.id.btndiv).setOnClickListener(this);
        findViewById(R.id.btnclr).setOnClickListener(this);
        findViewById(R.id.btneq).setOnClickListener(this);
```  
3. 根据点击按钮的侦听事件，确定播放哪个音频文件
```  
public void onClick(View v) {
        switch(v.getId()){
            case R.id.btn0:
                textView1.append("0");  //在UI界面的TextView中显示 0
                sp.play(spMap.get("0"), 1, 1, 0, 0, 1); //播放音频流
                break;
            case R.id.btn1:
                textView1.append("1");
                sp.play(spMap.get("1"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn2:
                textView1.append("2");
                sp.play(spMap.get("2"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn3:
                textView1.append("3");
                sp.play(spMap.get("3"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn4:
                textView1.append("4");
                sp.play(spMap.get("4"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn5:
                textView1.append("5");
                sp.play(spMap.get("5"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn6:
                textView1.append("6");
                sp.play(spMap.get("6"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn7:
                textView1.append("7");
              sp.play(spMap.get("7"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn8:
                textView1.append("8");
                sp.play(spMap.get("8"), 1, 1, 0, 0, 1);
                break;
            case R.id.btn9:
                textView1.append("9");
                sp.play(spMap.get("9"), 1, 1, 0, 0, 1);
              break;
            case R.id.btnadd:
                sp.play(spMap.get("plus"), 1, 1, 0, 0, 1);
                items.add(new Item(Double.parseDouble(textView1.getText().toString()), Type.num));
                checkAndcompute();
                items.add(new Item(0, Type.add));
                textView1.setText("");
                break;
            case R.id.btnsub:
                sp.play(spMap.get("minus"), 1, 1, 0, 0, 1);
                items.add(new Item(Double.parseDouble(textView1.getText().toString()), Type.num));
                checkAndcompute();
                items.add(new Item(0, Type.sub));
                textView1.setText("");
                break;
            case R.id.btnmul:
                sp.play(spMap.get("mul"), 1, 1, 0, 0, 1);
                items.add(new Item(Double.parseDouble(textView1.getText().toString()), Type.num));
                checkAndcompute();
                items.add(new Item(0, Type.mul));
                textView1.setText("");
                break;
          case R.id.btndiv:
                sp.play(spMap.get("div"), 1, 1, 0, 0, 1);
                items.add(new Item(Double.parseDouble(textView1.getText().toString()), Type.num));
                checkAndcompute();
                items.add(new Item(0, Type.div));
                textView1.setText("");
                break;
            case R.id.btneq:
                sp.play(spMap.get("equal"), 1, 1, 0, 0, 1);
                items.add(new Item(Double.parseDouble(textView1.getText().toString()), Type.num));
                checkAndcompute();
                textView1.setText(items.get(0).value + "");
                String str = items.get(0).value+"";
                new Test(str).start();  // 启动另一个线程来播放结果
                items.clear();
                break;
            case R.id.btnclr:
                sp.play(spMap.get("ac"), 1, 1, 0, 0, 1);
                textView1.setText("");
        }
  }
```  
4. 对于结算结果的音频播放：（1）先把计算结果转成字符串型，再启动一个子线程来处理该字符串；（2）子线程获取传过来的计算结果的字符串后，停止播放的“等于”对应的音频，然后利用for循环语句和String类的substring()方法把计算结果一位一位的截取出来，并播放对应的音频文件。
```  
                .
                .
                String str = items.get(0).value+"";
              new Test(str).start();  // 启动另一个线程来播放结果
                .
              .
class Test extends Thread{
        private String result;  //计算的结果
        public Test(String s){
            this.result = s;
        }
        @Override
        public void run() {
          String s;
            sp.stop(spMap.get("equal"));
            // 用for循环把计算结果分割成对应spMap中的key
            for(int i=0; i<result.length(); i++){
                try {
                    Thread.sleep(350);  //该线程睡350毫秒
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                s = result.substring(i, i+1);
                sp.play(spMap.get(s), 1, 1, 0, 0, 1);
            }
        }
    }
