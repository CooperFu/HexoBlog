title: Material Design系列之 全新的动画
date: 2014-07-03 16:22:53
tags: Material Design
---
## 全新的动画

在`Material Design`设计中，为用户与app交互反馈他们的动作行为和提供了视觉上的连贯性。Material主题为控件和Activity的过渡提供了一些默认的动画，在android L上，允许自定义这些动画：

*   `Touch feedback` 触摸反馈
*   `Circular Reveal` 圆形展示
*   `Curved motion` 曲线运动
*   `View state changes` 视图状态变化
*   `Vector Drawables` 矢量图动画
*   `Activity transitions` 活动转场

<a id="more"></a>

## 触摸反馈

触摸反馈是指用户在触摸控件时的一种可视化交互，在Android L之前，通常是通过press色变来凸显，但是因为是瞬间变化的效果，不如动画生动。

在Android L使用了`RippleDrawable`类，用一个水波纹扩散效果在两种不同的状态间过渡。

使用`Material Design`样式的应用，`button`默认带有该效果。除了默认的效果外，系统还提供了另外两种效果，我们只把`button`的背景指定为：

    ?android:attr/selectableItemBackground
    ?android:attr/selectableItemBackgroundBorderless

任何`view`处于可点击状态，都可以使用`RippleDrawable`来达到水波纹特效。

我们也可以通过设置`RippleDrawable`的颜色属性来调节动画颜色，系统默认的颜色为主题的一个属性颜色：`android:colorControlHighlight`，所以我们可以通过修改该颜色值来统一修改默认的水波纹颜色。`android:colorAccent`可以修改`checkbox`的选中颜色，更多颜色设置请参考主题。

系统的三种触摸反馈都是通过xml构建的，内容如下：

## 默认：
	<ripple xmlns:android="http://schemas.android.com/apk/res/android"
	        android:color="?android:attr/colorControlHighlight">
	    <item>
	        <inset
	               android:insetLeft="4dp"
	               android:insetTop="6dp"
	               android:insetRight="4dp"
	               android:insetBottom="6dp">
	            <shape android:shape="rectangle">
	                <corners android:radius="2dp" />
	                <solid android:color="?android:attr/colorButtonNormal" />
	                <padding android:left="8dp"
	                         android:top="4dp"
	                         android:right="8dp"
	                         android:bottom="4dp" />
	            </shape>
	        </inset>
	    </item>
		</ripple>
		
	?android:attr/selectableItemBackground
		
	<ripple xmlns:android="http://schemas.android.com/apk/res/android"
	        android:color="?android:attr/colorControlHighlight">
	    <item android:id="@android:id/mask">
	        <color android:color="@android:color/white" />
	    </item>
		</ripple>
	?android:attr/selectableItemBackgroundBorderless
		
	<ripple xmlns:android="http://schemas.android.com/apk/res/android"
		android:color="?android:attr/colorControlHighlight"/>

## 圆形展示

我们通常会显示或者隐藏一个view，在Android L之前，这是一个生硬瞬间变化动作，现在，有了一个新的api为此效果提供一个圆形的显示或者隐藏的动画效果。

`RevealAnimator`和之前的动画使用没什么区别，同样可以设置监听器和加速器来实现各种各样的特效，该动画主要用在隐藏或者显示一个view，改变view的大小等过渡效果。

通过`ViewAnimationUtils.createCircularReveal`来创建一个动画，该api接受5个参数：

*   `view` 操作的视图
*   `centerX` 动画开始的中心点X
*   `centerY` 动画开始的中心点Y
*   `startRadius` 动画开始半径
*   `startRadius` 动画结束半径

#### 沿着中心的缩小的动画
	Animator animator = ViewAnimationUtils.createCircularReveal(view, view.getWidth() / 2, view.getHeight() / 2, view.getWidth(), 0);
		animator.setInterpolator(new LinearInterpolator());
		animator.setDuration(1000);
		animator.start();

#### 从左上角扩展的圆形动画
	Animator animator = ViewAnimationUtils.createCircularReveal(view,0,0,0,(float) Math.hypot(view.getWidth(), view.getHeight()));
	animator.setDuration(1000);
	animator.start();

#### 曲线运动

曲线动画在Android L之前我们可以通过继承位移动画重载`applyTransformation`函数来实现运动轨迹算法，但是操作起来比较繁琐：

通过继承位移动画，来改写`applyTransformation`来修改位移的轨迹

通过继承位移动画，来改写`applyTransformation`来修改位移的轨迹

	public class ArcTranslateAnimation extends Animation {
    private float mFromXValue,mToXValue,mFromYValue,mToYValue;
    private float mFromXDelta,mToXDelta,mFromYDelta,mToYDelta;
    private PointF mStart,mControl,mEnd;
    public ArcTranslateAnimation(float fromXValue, float toXValue, float fromYValue, float toYValue) {
        mFromXValue = fromXValue;
        mToXValue = toXValue;
        mFromYValue = fromYValue;
        mToYValue = toYValue;
    }
    protected void applyTransformation(float interpolatedTime, Transformation t) {
        float dx = calcBezier(interpolatedTime, mStart.x, mControl.x, mEnd.x);
        float dy = calcBezier(interpolatedTime, mStart.y, mControl.y, mEnd.y);
        t.getMatrix().setTranslate(dx, dy);
    }
    public void initialize(int width, int height, int parentWidth, int parentHeight) {
        super.initialize(width, height, parentWidth, parentHeight);
        mFromXDelta = resolveSize(ABSOLUTE, mFromXValue, width, parentWidth);
        mToXDelta = resolveSize(ABSOLUTE, mToXValue, width, parentWidth);
        mFromYDelta = resolveSize(ABSOLUTE, mFromYValue, height, parentHeight);
        mToYDelta = resolveSize(ABSOLUTE, mToYValue, height, parentHeight);
        mStart = new PointF(mFromXDelta, mFromYDelta);
        mEnd = new PointF(mToXDelta, mToYDelta);
        mControl = new PointF(mFromXDelta, mToYDelta);
    }
    private long calcBezier(float interpolatedTime, float p0, float p1, float p2) {
        return Math.round((Math.pow((1 - interpolatedTime), 2) * p0) 
        + (2 * (1 - interpolatedTime) * interpolatedTime * p1) + 
        (Math.pow(interpolatedTime, 2) * p2);
    }
}

## 矢量图动画

前面我们学习了矢量图，`AnimatedVectorDrawable`类让你能使一个矢量图动起来。矢量图动画比帧动画更平滑的展现图片的变化过程，并且无论在内存占用，还是包体积占用上都要优于帧动画。通常定义一个矢量图动画需要三步：

在`drawable`资源目录下定义一个矢量图

	<vector xmlns:android="http://schemas.android.com/apk/res/android"
	    android:height="64dp"
	    android:width="64dp"
	    android:viewportHeight="600"
	    android:viewportWidth="600">
	    <group
	        android:name="rotationGroup"
	        android:pivotX="300.0"
	        android:pivotY="300.0"
	        android:rotation="45.0" >
	        <path
	            android:name="v"
	            android:fillColor="#000000"
	            android:pathData="M300,70 l 0,-70 70,70 0,0 -70,70z" />
	    </group>
	</vector>

>

## 转场动画



## 普通转场动画

所有继承自`visibility`类都可以作为进入、退出的过度动画。如果我们想自定义进入和退出时的动画效果，只需要继承`Visibility`，重载`onAppear`和`onDisappear`方法来定义进入喝退出的动画。系统提供了三种默认方式：

*   `explode` 从屏幕中心移入或移出视图
*   `slide` 从屏幕边缘移入或移出视图
*   `fade` 改变视图的透明度

想在xml中指定自定义的进入、退出的过度动画需要先对动画进行定义：

`<transition class="my.app.transition.CustomTransition"/>`

注意：其中`CustomTransition`是你自定义的动画，它必须继承自`Visibility`。

想以普通转场动画的方式启动一个Activity，必须在`startActivity`函数中传递一个对象:
	
	ActivityOptions options = ActivityOptions.makeSceneTransitionAnimation(activity);  
	startActivity(intent, options.toBundle());

如果想让返回也具备转场效果，那么在返回的Activity中不要再调用`finish`函数，而是应该使用`finishAfterTransition`来结束一个`Activity`，该函数会等待动画执行完毕才结束该Activity。

## 共享转场动画

当两个`Activity`具备某些相遇的元素时，共享转场动画将是一个非常好的选择。使用转场动画需要将相同的元素通过`android:transitionName`或者`view.setTransitionName`设置为相同的名称，这样系统才能区分出相同的元素。

#### 共享转场动画支持以下共享元素：

*   `changeBounds` 对目标视图的大小进行动画
*   `changeClipBounds` 对目标视图的剪裁大小进行动画
*   `changeTransform` 对目标视图进行缩放、旋转、位移动画
*   `changeImageTransform` 对目标图片进行缩放

#### 通过下面的函数启动一个共享元素动画：
	ActivityOptions options = ActivityOptions.makeSceneTransitionAnimation(activity, view, "name");  
	startActivity(intent, options.toBundle());

#### 如果有多个共享元素，则可以通过Pair进行包装处理：
	

	ActivityOptions options = ActivityOptions.makeSceneTransitionAnimation(activity,Pair.create(view1, "name1"),Pair.create(view2, "name2"));      
	startActivity(intent,.toBundle());

返回时如果需要具备转场动画，那么也需要用`finish`函数替代`finishAfterTransition`来结束一个Activity。

共享转场动画通常可以根据指定的元素判断出合适的转场动画效果，不需要我们做额外的处理，也可以通过之前学习的方法进行指定共享元素转场动画效果。

## 组合转场动画

我们可以把多个转场动画进行组合，作出更具个性的转场效果，在资源文件中通过以下方式：

代码中我们可以通过`TransitionSet`类组合多个转场动画：

组合可以同时针对普通转场动画和共享元素转场动画。

转场动画也可以像普通动画一样设置持续时间，延期执行时间，速率插入器，以及动画的监听等。

转场动画通常是对整个布局起作用，如果我们想对某个特定的view实施转场动画，可以把该`view`设置为转场动画的`target`，这样转场动画将只对特定的view起作用。共享元素的动画的`target`需要指定为`transitionName`.