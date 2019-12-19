# SlidingPaneLayout-Left-Right-
SlidingPaneLayout_Left&amp;right_安卓左滑&amp;右滑关闭
![效果](https://github.com/DecentChunibyoPatient/SlidingPaneLayout-Left-Right-/blob/master/1.gif)

#### 主要代码
package cf.paradoxie.swipebackdemo;  
/**  
* Created by xiehehe on 2016/10/28.  
*/  
import android.annotation.SuppressLint;  
import android.app.Activity;  
import android.os.Build;  
import android.os.Bundle;  
import android.support.annotation.NonNull;  
import android.support.v4.view.ViewCompat;  
import android.support.v4.widget.SlidingPaneLayout;  
import android.support.v7.app.AppCompatActivity;  
import android.view.View;  
import android.view.ViewGroup;  
import java.lang.reflect.Field;  
/**  
* User: xiehehe  
* Date: 2016-10-28  
* Time: 22:32  
* FIXME  
*/  
public class BaseActivity extends Activity implements SlidingPaneLayout.PanelSlideListener {  
@Override  
protected void onCreate(Bundle savedInstanceState) {  
//初始化  
initSwipeBackFinish();  
super.onCreate(savedInstanceState);  
}  
@Override  
public void onPanelSlide(View panel, float slideOffset) {  
}  
/**  
* 显示隐藏视图的事件  
* @param view  
*/  
@Override  
public void onPanelOpened(View view) {  
finish();  
this.overridePendingTransition(0, R.anim.slide_out_right);  
}  
/**  
* 关闭事件  
* @param view  
*/  
@Override  
public void onPanelClosed(@NonNull View view) {  
}  
/**  
* 初始化滑动返回  
*/  
private void initSwipeBackFinish() {  
if (isSupportSwipeBack()) {  
ViewGroup decor = (ViewGroup) getWindow().getDecorView();  
decor.addView(getSlidingPaneLayout(getSlidingPaneLayoutMV(), getSlidingPaneLayout(getSlidingPaneLayoutMV(), (ViewGroup) removeView(decor,0), ViewCompat.LAYOUT_DIRECTION_LTR),ViewCompat.LAYOUT_DIRECTION_RTL));  
}  
}  
/**  
* 把一个视图移出来  
* @param viewGroup  
* @param i  
* @return  
*/  
View removeView(ViewGroup viewGroup,int i){  
View view=viewGroup.getChildAt(i);  
view.setBackgroundColor(getResources().getColor(android.R.color.white));  
viewGroup.removeView(view);  
return view;  
}  
/**  
* 一个很随便的视图  
* @return  
*/  
View getSlidingPaneLayoutMV(){  
View view = new View(this);  
view.setLayoutParams(new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT));  
return view;  
}  
/**  
*  
* @param view  隐藏视图  
* @param decorChild 显示视图  
* @param layoutDirection 显示隐藏视图的滑动手势  
* @return  
*/  
@SuppressLint("WrongConstant")  
SlidingPaneLayout getSlidingPaneLayout(View view,ViewGroup decorChild,int layoutDirection ){  
SlidingPaneLayout slidingPaneLayout =  new SlidingPaneLayout(this);  
//通过反射改变mOverhangSize的值为0，这个mOverhangSize值为菜单到右边屏幕的最短距离，默认  
//是32dp，现在给它改成0  
try {  
//属性  
Field f_overHang = SlidingPaneLayout.class.getDeclaredField("mOverhangSize");  
f_overHang.setAccessible(true);  
f_overHang.set(slidingPaneLayout, 0);  
} catch (Exception e) {  
e.printStackTrace();  
}  
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {  
slidingPaneLayout.setLayoutDirection(layoutDirection);  
}  
slidingPaneLayout.setPanelSlideListener(this);  
slidingPaneLayout.setSliderFadeColor(getResources().getColor(android.R.color.transparent));  
slidingPaneLayout.addView(view, 0);  
slidingPaneLayout.addView(decorChild, 1);  
return slidingPaneLayout;  
}  
/**  
* 是否支持滑动返回  
*  
* @return  
*/  
protected boolean isSupportSwipeBack() {  
return true;  
}  
}  
