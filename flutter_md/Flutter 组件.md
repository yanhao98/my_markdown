# 组件整理网站

http://laomengit.com/



# MaterialApp

## 属性: theme

```dart
new ThemeData(
        primarySwatch: Colors.blue,
      )
```

## 属性: home



# Scaffold

Creates a visual scaffold for material design widgets.

## 属性: resizeToAvoidBottomInset

## 属性: resizeToAvoidBottomPadding



## 属性: appBar

可以用`PreferredSize`



## 属性: body









# 布局组件

## W: Container

```dart
Container({
    Key key,
    this.alignment,
    this.padding,
    Color color,
    Decoration decoration,
    this.foregroundDecoration,
    double width,
    double height,
    BoxConstraints constraints,
    this.margin,
    this.transform,
    this.child,
  })
```

### constraints

大小约束

```dart
  const BoxConstraints({
    this.minWidth = 0.0,
    this.maxWidth = double.infinity,
    this.minHeight = 0.0,
    this.maxHeight = double.infinity,
  });
```



### decoration

```dart
const BoxDecoration({
    this.color, // 填充颜色
    this.image,
    this.border,  // 边框
    this.borderRadius, // 圆角
    this.boxShadow, // 阴影
    this.gradient, // 渐变填充
    this.backgroundBlendMode,
    this.shape = BoxShape.rectangle,
  })
```

#### border

```dart
Border({
  this.top = BorderSide.none,
  this.right = BorderSide.none,
  this.bottom = BorderSide.none,
  this.left = BorderSide.none,
})
```

```dart
Border.all({
  Color color = const Color(0xFF000000),
  double width = 1.0,
  BorderStyle style = BorderStyle.solid,
})
```



## W: Center

## W: Column、Row

继承自 Flex

```dart
//主轴方向，Column的竖向、Row我的横向
mainAxisAlignment: MainAxisAlignment.start, 
//默认是最大充满、还是根据child显示最小大小
mainAxisSize: MainAxisSize.max,
//副轴方向，Column的横向、Row我的竖向
crossAxisAlignment :CrossAxisAlignment.center,
```

### W: Expanded

在 Column 和  Row 中代表着平均充满的作用，当有两个存在的时候默认均分充满。同时页可以设置 `flex` 属性决定比例。（flex默认=1）

### W: Spacer

*包装了一个 Expanded 的 SizedBox 。*
等同于

```dart
Expanded(
  flex: flex,
  child: const SizedBox.shrink(),
);
```



## W: AspectRatio

让child按照比例（长宽比）显示。

```dart
const AspectRatio({
    Key key,
    @required this.aspectRatio,
    Widget child,
  })
```



## W: Stack

```dart
Stack({
  Key key,
  this.alignment = AlignmentDirectional.topStart,
  this.textDirection,
  this.fit = StackFit.loose,
  this.overflow = Overflow.clip,
  List<Widget> children = const <Widget>[],
})
```



## W: Positioned

```dart
Positioned({
  Key key,
  this.left,
  this.top,
  this.right,
  this.bottom,
  this.width,
  this.height,
  @required Widget child,
})
```



## W: Transform

先平移再旋转和先旋转再平移的区别。

https://jingyan.baidu.com/article/414eccf617a9c66b421f0a5e.html



## W: LimitedBox

```dart
LimitedBox(
	maxHeight: 48,child:,
)
```



## W: AspectRatio



## W: LayoutBuilder

```dart
LayoutBuilder({
  Key key,
  @required LayoutWidgetBuilder builder,
})
```
### LayoutWidgetBuilder

```dart
typedef LayoutWidgetBuilder = Widget Function(BuildContext context, BoxConstraints constraints);
```



# W: CustomScrollView

https://segmentfault.com/a/1190000019902201

- `SliverAppBar`：Creates a material design app bar that can be placed in a CustomScrollView.
- `SliverPersistentHeader`：Creates a sliver that varies its size when it is scrolled to the start of a viewport.
- `SliverFillRemaining`：Creates a sliver that fills the remaining space in the viewport.
- `SliverToBoxAdapter`：Creates a sliver that contains a single box widget.
- `SliverPadding`：Creates a sliver that applies padding on each side of another sliver.

 ## SliverToBoxAdapter

> Creates a sliver that contains a single box widget.



## SliverGrid

```dart
SliverGrid(
  delegate: SliverChildBuilderDelegate(
    (BuildContext context, int index) {
      print('渲染 Grid：$index');
      return Container();
    },
  ),
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 2,
    childAspectRatio: 16 / 16,
    crossAxisSpacing: 8.0,
    mainAxisSpacing: 8.0,
  ),
)
```

### 参数: delegate

- 值: SliverChildBuilderDelegate()

  - ```dart
    SliverChildBuilderDelegate(
      (BuildContext context, int index) {
        print('渲染 Grid：$index');
        return Container();
      },
    )
    ```

- 值: SliverChildListDelegate()

  - ```dart
    SliverChildListDelegate(
      this.children, {
      this.addAutomaticKeepAlives = true,
      this.addRepaintBoundaries = true,
      this.addSemanticIndexes = true,
      this.semanticIndexCallback = _kDefaultSemanticIndexCallback,
      this.semanticIndexOffset = 0,
    })
    ```

### 参数: gridDelegate

> The delegate that controls the size and position of the children.

- 值: SliverGridDelegateWithFixedCrossAxisCount()
- 值: SliverGridDelegateWithMaxCrossAxisExtent()



# 组件

## W: Text

```
TextStyle(
  //不继承默认字体样式
  inherit: false,
  color: Colors.blue
  )
```

```dart
DefaultTextStyle( // DefaultTextStyle() 包裹设置默认字体样式
  style:TextStyle(),
  Column(
  	children:[
      Text('Hello',
        style: Theme.of(context).textTheme.display1,
      ),
      Text('Hello',
        style: TextStyle(
          inherit: false,//不继承默认字体样式
          color: Colors.blue
        ),
      ),
    ]
  )
)
```

### style

```dart
TextStyle(
  fontSize: 24.0,
  color: Colors.redAccent,
  decorationStyle: TextDecorationStyle.dotted,
  fontWeight: FontWeight.bold,
  fontStyle: FontStyle.italic,
  decoration: TextDecoration.underline)
```



## W: CircularProgressIndicator

```dart
const CircularProgressIndicator({
    Key key,
    double value,
    Color backgroundColor,
    Animation<Color> valueColor,
    this.strokeWidth = 4.0,
    String semanticsLabel,
    String semanticsValue,
  })
```

### valueColor

类型是`Animation<color>`，不需要对进度条颜色执行动画，可以通过`AlwaysStoppedAnimation`来指定。

```dart
AlwaysStoppedAnimation(Theme.of(context).accentColor)
```



## W: Offstage

*Creates a widget that visually hides its child.*

```dart
const Offstage({ Key key, this.offstage = true, Widget child })
```

### **offstage**

默认为true，也就是不显示，当为flase的时候，会显示该控件。



## W: TextField

### 参数：decoration

```dart
final InputDecoration decoration;
```

```dart
InputDecoration({
    this.icon,    //位于装饰器外部和输入框前面的图片
    this.labelText,  //用于描述输入框，当输入框获取焦点时默认会浮动到上方，
    this.labelStyle,  // 控制labelText的样式,接收一个TextStyle类型的值
    this.helperText, //辅助文本，位于输入框下方，如果errorText不为空的话，则helperText不会显示
    this.helperStyle, //helperText的样式
    this.hintText,  //提示文本，位于输入框内部
    this.hintStyle, //hintText的样式
    this.hintMaxLines, //提示信息最大行数
    this.errorText,  //错误信息提示
    this.errorStyle, //errorText的样式
    this.errorMaxLines,   //errorText最大行数
    this.hasFloatingPlaceholder = true,  //labelText是否浮动，默认为true，修改为false则labelText在输入框获取焦点时不会浮动且不显示
    this.isDense,   //改变输入框是否为密集型，默认为false，修改为true时，图标及间距会变小
    this.contentPadding, //内间距
    this.prefixIcon,  //位于输入框内部起始位置的图标。
    this.prefix,   //预先填充的Widget,跟prefixText同时只能出现一个
    this.prefixText,  //预填充的文本，例如手机号前面预先加上区号等
    this.prefixStyle,  //prefixText的样式
    this.suffixIcon, //位于输入框后面的图片,例如一般输入框后面会有个眼睛，控制输入内容是否明文
    this.suffix,  //位于输入框尾部的控件，同样的不能和suffixText同时使用
    this.suffixText,//位于尾部的填充文字
    this.suffixStyle,  //suffixText的样式
    this.counter,//位于输入框右下方的小控件，不能和counterText同时使用
    this.counterText,//位于右下方显示的文本，常用于显示输入的字符数量
    this.counterStyle, //counterText的样式
    this.filled,  //如果为true，则输入使用fillColor指定的颜色填充
    this.fillColor,  //相当于输入框的背景颜色
    this.errorBorder,   //errorText不为空，输入框没有焦点时要显示的边框
    this.focusedBorder,  //输入框有焦点时的边框,如果errorText不为空的话，该属性无效
    this.focusedErrorBorder,  //errorText不为空时，输入框有焦点时的边框
    this.disabledBorder,  //输入框禁用时显示的边框，如果errorText不为空的话，该属性无效
    this.enabledBorder,  //输入框可用时显示的边框，如果errorText不为空的话，该属性无效
    this.border, //正常情况下的border
    this.enabled = true,  //输入框是否可用
    this.semanticCounterText,  
    this.alignLabelWithHint,
  })
```







## W: ListView

https://blog.csdn.net/zgcqflqinhao/article/details/85121054

### 参数: *physics*

设置 ListView 如何响应用户的滑动行为。

- AlwaysScrollableScrollPhysics：总是可以滑动。
- NeverScrollableScrollPhysics：禁止滚动。



## W: InkWell

![InkWell](https://tva1.sinaimg.cn/large/00831rSTgy1gdgag60aq5g309o07waat.gif)





## W: ListWheelScrollView

![](https://tva1.sinaimg.cn/large/00831rSTgy1gdjtudyhg1j30dw0ozt9g.jpg)







# GestureDetector

*检测手势*



# 路由组件

路由原理：https://juejin.im/post/5c8db8e8f265da2d864b889f



## W: [ModalRoute](https://api.flutter.dev/flutter/widgets/ModalRoute/maintainState.html)

### 参数：maintainState = true

​	路由处于非活动状态时是否应保留在内存中。



screen1 push 到 screen2

```dart
Navigator.of(context).push(MaterialPageRoute(
  builder: (context) => SecondScreen(),
  maintainState: false,
));
```

screen2 push 到 screen3

```dart
Navigator.of(context).push(MaterialPageRoute(
  builder: (context) => ThirdScreen(),
));
```

此时：

1. 从screen3 pop 到 screen2。
2. screen2 会执行 initState
3. 从screen2 pop 到screen1
4. screen1 不会执行 initState
