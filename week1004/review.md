## ARTS - Review
## [Kotlin 可触摸式自定义动画视图](https://medium.com/@elye.project/custom-touchable-animated-view-in-kotlin-3ad599f85dbc)

![](https://cdn-images-1.medium.com/max/1600/0*cboYNHmYwpWUQ75x.jpg)

如果你想绘画自己的视图，并且有一些动画，kotlin可以帮你。

### 多点触控动画增长圆圆 (类似下雨)
下面就是我将会展示的怎么做这样一个视图。

![](https://cdn-images-1.medium.com/max/1600/1*AFVNkiVdTB7Fw949OV1_8g.gif)

它包含：
1. 多点触控能力。 每个点击将会画一个园。
2. 圆圈将会增长，颜色淡入，直到消失

### 制作自定义视图

#### 1. 实现视图

首先你需要实现 View 类, 它是android 基本UI组件

```


class RainDropView @JvmOverloads constructor(
        context: Context,
        attrs: AttributeSet? = null,
        defStyleAttr: Int = 0,
        defStyleRes: Int = 0) :
        View(context, attrs, defStyleAttr, defStyleRes)
        
```
Kotlin 好的部分是，你可以用默认构造器把所有构造器合并成一个。

*了解跟多xml属性设置，参考我的另一个博客 [kotlin自定义组件](https://medium.com/@elye.project/building-custom-component-with-kotlin-fc082678b080)*

#### 2. 定义 onMeasure
定义自己的视图的宽高，下面提供一个相对基础的模型。

```

override fun onMeasure(
    widthMeasureSpec: Int, heightMeasureSpec: Int) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec)
    val desiredWidth = suggestedMinimumWidth + 
                       paddingLeft + paddingRight
    val desiredHeight = suggestedMinimumHeight + 
                       paddingTop + paddingBottom
    setMeasuredDimension(
            resolveSize(desiredWidth, widthMeasureSpec),
            resolveSize(desiredHeight, heightMeasureSpec))
}

```

了解更多请参考[自定义视图：onMeasure](https://medium.com/@quiro91/custom-view-mastering-onmeasure-a0a0bb11784d)

#### 3. 画图，加动画
用 onDraw 方法实现画图

```

override fun onDraw(canvas: Canvas) {
    super.onDraw(canvas)
    rainDropList.forEach { it.draw(canvas, paint) }
    rainDropList = rainDropList.filter { it.isValid() }
    if (rainDropList.isNotEmpty() && isAttachedToWindow) {
        invalidate()
    }
}

```

像普通Java 编程，我们用提供的 canvas 画图。
我用 RainDrop 类来封装绘画圆圈和圆圈增长的逻辑。

```
rainDropList.forEach { it.draw(canvas, paint) }

```

之后检查 rainDrop 是否到了最大尺寸，就移除，使用：

```
rainDropList = rainDropList.filter { it.isValid() }

```

最后，保持动画反复调用自身的invalidate方法来重新画自己。 这种调用只出现在 rainDrop 在生效中和视图被触发时候。

```

if (rainDropList.isNotEmpty() && isAttachedToWindow) {
    invalidate()
}

```

#### 4. 处理触摸

最后，我们处理触摸事件，

```

@SuppressLint("ClickableViewAccessibility")
override fun onTouchEvent(event: MotionEvent): Boolean {
    val pointerIndex = event.actionIndex
    when (event.actionMasked) {
        MotionEvent.ACTION_DOWN,
        MotionEvent.ACTION_POINTER_DOWN -> return true
        MotionEvent.ACTION_UP,
        MotionEvent.ACTION_POINTER_UP -> {
            rainDropList += RainDrop(
                                event.getX(pointerIndex), 
                                event.getY(pointerIndex), maxRadius)
            invalidate()
            return true
        }
    }
    return super.onTouchEvent(event)
}

```

注意，我们需要处理 ACTION_DOWN 和 ACTION_POINTER_DOWN (通过返回true), 我们不需要处理 ACTION_UP 和 ACTION_POINTER_UP.

在 ACTION_UP 和 ACTION_POINTER_UP (通常用来多点触控)， 我们创建一个新的 RainDrop 类 来每次识别 XY 坐标位置。

我们调用 invalidate 来触发重新画图。

**代码 在Github 地址 [android raindrop](https://github.com/elye/demo_android_raindrop_view)**

