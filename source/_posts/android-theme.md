---
title: 安卓沉浸式状态栏
date: 2017-11-03 14:10:14
tags:
---
# 安卓沉浸式状态栏的全解析
***
## 1.主题中设置

```
//主题设置
values <19
<!--透明的主题设置-->
<style name="StatusTranslucentTheme" parent="AppTheme">
    <!--在Android 4.4之前的版本上运行，直接跟随系统主题-->
</style>

values-v19 5.0
<style name="StatusTranslucentTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
</style>

values-v21 6.0
<style name="StatusTranslucentTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="android:windowTranslucentStatus">false</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <!--Android 5.x开始需要把颜色设置透明，否则导航栏会呈现系统默认的浅灰色-->
        <item name="android:statusBarColor">@android:color/transparent</item>
</style>

```


```
public class ToolBarUi extends AppCompatActivity {

    View parentView;
    @BindView(R.id.toolbar)
    Toolbar mToolbar;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_tool_bar_ui);
        ButterKnife.bind(this);
        mToolbar.setTitle("");
        //setSupportActionBar(mToolbar);
        //获取到该界面的根布局（Framelayout）
        ViewGroup contentFrameLayout = (ViewGroup) findViewById(Window.ID_ANDROID_CONTENT);
        //获取根布局下的第一个view（不建议使用merge的布局使用）
        parentView = contentFrameLayout.getChildAt(0);
        if (parentView != null && Build.VERSION.SDK_INT >= 14) {
            //代码添加属性，也可以在xml文件中进行设置
            parentView.setFitsSystemWindows(true);
        }
    }

    //更换背景和toolbar的颜色
    private void setParentBackgroudColor(int color) {
        if (parentView != null) {
            //设置背景色（状态栏的蓝色会随着背景作出相应的改变）
            parentView.setBackgroundColor(color);
            //改变toobar的颜色保持两者的一致
            mToolbar.setBackgroundColor(color);
        }
    }
}
```
## 2.代码中设置


