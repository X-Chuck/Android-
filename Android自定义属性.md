# Android 自定义属性 #

1. 在attrs.xml文件中声明属性
		
		<declare-styleable name="MyToggleBtn">            // 声名属性集的名称，即这些属性是属于哪个控件的。  
		<attr name="current_state" format="boolean"/>   // 声名属性 current_state 格式为 boolean 类型  
		<attr name="slide_button" format="reference"/>   // 声名属性 slide_button格式为 reference 类型  
		</declare-styleable> 
所用的format类型

		reference     	引用
		color           颜色
		boolean       	布尔值
		dimension  		尺寸值
		float           浮点值
		integer        	整型值
		string          字符串
		enum          	枚举值
2. 复写第二个构造函数，并解析属性
		
    	public MyView(Context context, AttributeSet attrs) {  
        	super(context, attrs);  
			currentState = ta.getColor(R.styleable.MyToggleBtn_current_state, false);
			slideButton = ta.getColor(R.styleable.MyToggleBtn_current_state, ...);
		}
3. 在布局文件中声明命名空间，然后使用属性


	    <com.example.myattrs.MyView  
    		xmlns:example="http://schemas.android.com/apk/res/com.example.myattrs"  
	        android:layout_width="wrap_content"  
	        android:layout_height="wrap_content"  
	        android:layout_centerInParent="true"  
	        example:test_bitmap="@drawable/ic_launcher"  
	        example:test_msg="@string/app_name" />  