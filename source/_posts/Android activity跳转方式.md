---
title: Android activity跳转方式
date: 2016-03-05 16:34:19
---
方法一：通过SetContentView切换Layout来实现界面的切换，这种方法相当于重绘Activity.

	protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	
	        Button btnInsert = (Button) this.findViewById(R.id.btnInsert);    //获取btn
	        btnInsert.setOnClickListener(new View.OnClickListener() {    //添加监听器
	            @Override
	            public void onClick(View v) {
	                setContentView(R.layout.activity_insert);        //跳转
	            }
	        });
	    }
方法二：在一个程序中使用Intent对象来指定一个Activity，并通过startActivity方法启动这个Activity.
	protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	
	        Button btnInsert = (Button) this.findViewById(R.id.btnInsert);    //获取按钮
	        btnInsert.setOnClickListener(new View.OnClickListener() {    //添加监听器
	            @Override
	            public void onClick(View v) {
	                Intent intent = new Intent();
	                intent.setClass(MainActivity.this, InsertActivity.class);    //设置Intent属性
	                MainActivity.this.startActivity(intent);    //跳转
	            }
	        });
	    }