Android ViewPagerIndicator
==========================

Android Viewpager Indicator是Android开发中最常用的控件之一，几乎所有的新闻类APP中都有使用，下面介绍其基本使用方法。



Usage
=====

1. ViewPager Indicator的Library

查看Viewpager Indicator的Library代码，可以看到此项目的设计思想：
首先定义了一个PageIndicator接口，它里面定义了最重要和基本的indicator表现出的一些方法：
    1.1 首先一个indicator必须要与一个ViewPager关联在一起，所以它提供了一个setViewPager方法。
    1.2 它扩展了ViewPager.OnPageChangeListener接口，表示接管了ViewPager的Pager改变时的监听处理，
          这也是为什么为ViewPager设置OnPageChangeListener监听器时不能设置在ViewPager上而必须设置在
          indicator上的原因。
    1.3 还有一个notifyDataSetChanged通知方法，表示为这个ViewPager提供View(一般是Fragment)的  Adapter 里面的数据集发生变化时，执行的动作，这里可增加相关的逻辑。

2. Viewpager Indicator的实现类

 然后再看下Viewpager Indicator的实现类，共有6个，由6个类分别实现，它们分别为：
    2.1 小圆圈类型的
    2.2 带图标类型的                    
    2.3 小横线类型的，距离屏幕最下边端有一定的距离。
    2.4 标签类型的（Tab）
    2.5 标题类型的，与标签类型的有点像，但它当前的标题页的左/右边的标题会卷起，即往两端缩进去。
    2.6 屏幕底部小横线类型的，并且会占满整行。                    
                    

3. Viewpager Indicator随附带的Demo

    3.1 Demo项目的设计
    项目由一个ListSamples的ListActivity入口，它主要用作组装所有的子indicator的列表。
    TestFragment.java，所有ViewPager上真正显示的视图。
    TestFragmentAdapter.java，所有ViewPager里的Adapter，为ViewPager生成TestFragment。
    Samplexxx.java，所有的indicator的显示，一个类显示一种使用方法或特性。

3.2 具体使用方法
查看SampleCirclesDefault.java基本就可以明白它的基本使用方法：
首先，把Indicator包含进xml文件中，如下，注意它应该紧邻在ViewPager的上方或下方，总之要挨在一起。
    
    <com.viewpagerindicator.TitlePageIndicator
        android:id="@+id/titles"
        android:layout_height="wrap_content"
        android:layout_width="fill_parent" />

其次，使用titleIndicator.setViewPager(pager)把两者绑定在一起，如下所示： 
    
    //Set the pager with an adapter
     ViewPager pager = (ViewPager)findViewById(R.id.pager);
     pager.setAdapter(new TestAdapter(getSupportFragmentManager()));
     
     //Bind the title indicator to the adapter
     TitlePageIndicator titleIndicator = (TitlePageIndicator)findViewById(R.id.titles);
     titleIndicator.setViewPager(pager);
     
    最后，如果你要监听ViewPager中包含的Fragment的改变(手滑动切换了页面)，使用OnPageChangeListener为它指定一个监听器，那么不能直接设置在ViewPager上，而要设置在Indicator上，如下所示：
    
     //continued from above
     titleIndicator.setOnPageChangeListener(mPageChangeListener);
     
   4.修改indicator的样式（Theme） 
    4.1 Theme XML，在AndroidManifest.xml中相应的Activity中设置，比如： 
    
     <activity
            android:name=".SampleCirclesStyledTheme"
            android:label="Circles/Styled (via theme)"
            android:theme="@style/StyledIndicators">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="com.jakewharton.android.viewpagerindicator.sample.SAMPLE" />
            </intent-filter>
        </activity>
        
    android:theme="@style/StyledIndicators" ==> values/styles.xml中相应部分为： 
    
      <resources>
      <style name="StyledIndicators" parent="@android:style/Theme.Light">
          <item name="vpiCirclePageIndicatorStyle">@style/CustomCirclePageIndicator</item>
          <item name="vpiLinePageIndicatorStyle">@style/CustomLinePageIndicator</item>
          <item name="vpiTitlePageIndicatorStyle">@style/CustomTitlePageIndicator</item>
          <item name="vpiTabPageIndicatorStyle">@style/CustomTabPageIndicator</item>
          <item name="vpiUnderlinePageIndicatorStyle">@style/CustomUnderlinePageIndicator</item>
      </style>
      
    4.2 Layout XML，在Layout XML方法中指定，如下：
    
      <android.support.v4.view.ViewPager
          android:id="@+id/pager"
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1"
          />
      <com.viewpagerindicator.CirclePageIndicator
          android:id="@+id/indicator"
          android:padding="10dip"
          android:layout_height="wrap_content"
          android:layout_width="fill_parent"
          android:background="#FFCCCCCC"
          app:radius="10dp"
          app:fillColor="#FF888888"
          app:pageColor="#88FF0000"
          app:strokeColor="#FF000000"
          app:strokeWidth="2dp"
          />
          
    4.3    Object methods，直接在代码中设置，如下：
    
          CirclePageIndicator indicator = (CirclePageIndicator)findViewById(R.id.indicator);
          mIndicator = indicator;
          indicator.setViewPager(mPager);
   
          final float density = getResources().getDisplayMetrics().density;
          indicator.setBackgroundColor(0xFFCCCCCC);
          indicator.setRadius(10 * density);
          indicator.setPageColor(0x880000FF);
          indicator.setFillColor(0xFF888888);
          indicator.setStrokeColor(0xFF000000);
          indicator.setStrokeWidth(2 * density);
    
     具体有哪些属性可以参考library项目中的vpi__attrs.xml文件。
    
    
    
    
    
    
 
 
