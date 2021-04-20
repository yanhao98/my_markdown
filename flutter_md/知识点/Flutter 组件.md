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





## W: DecoratedBox

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



## W: (Sliver)LayoutBuilder

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



## W: Material

```dart
Material({
    Key key,
    this.type = MaterialType.canvas,
    this.elevation = 0.0,
    this.color,
    this.shadowColor = const Color(0xFF000000),
    this.textStyle,
    this.borderRadius,
    this.shape,
    this.borderOnForeground = true,
    this.clipBehavior = Clip.none,
    this.animationDuration = kThemeChangeDuration,
    this.child,
  })
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

## W: InkResponse

| 属性             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| highlightShape   | 如果形状为[BoxShape.circle](https://s0api0flutter0dev.icopy.site/flutter/painting/BoxShape-class.html) ，则高光将位于[InkResponse的](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse-class.html)中心. <br/>如果形状是[BoxShape.rectangle](https://s0api0flutter0dev.icopy.site/flutter/painting/BoxShape-class.html) ，则突出显示填充[InkResponse](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse-class.html) 。 |
| radius           | 墨水飞溅的半径<br/>默认情况下，此大小由[getRectCallback](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse/getRectCallback.html)提供的矩形的大小或[InkResponse](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse-class.html)**本身的大小**确定. |
| containedInkWell | 是否应限制此墨水响应范围.<br/>此标志还控制初始内容是否迁移到[InkResponse](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse-class.html)的中心. 如果[containedInkWell](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse/containedInkWell.html)为true，则飞溅将保持在抽头位置的中心. 如果为false，则启动[画面](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse-class.html)会随着[扩展](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse-class.html)而迁移到[InkResponse](https://s0api0flutter0dev.icopy.site/flutter/material/InkResponse-class.html)的中心. |
| borderRadius     | 墨水飞溅的圆角                                               |

### 不同 highlightShape 的区别

- BoxShape.circle
  - <img src="https://tva1.sinaimg.cn/large/00831rSTgy1gdngchi1rjg308g02a0u8.gif" alt="Apr-09-2020 13-30-05" style="zoom:100%;" align="left" />
- BoxShape.rectangle
  - <img src="https://tva1.sinaimg.cn/large/00831rSTgy1gdngcjo3trg308g02amyc.gif" alt="Apr-09-2020 13-29-34" style="zoom:100%;" align="left" />

### 不同 containedInkWell 的区别

- containedInkWell = true
  - <img src="https://tva1.sinaimg.cn/large/00831rSTgy1gdngnvn2qug308g02a759.gif" alt="Apr-09-2020 13-39-40" style="zoom:100%;" align="left" />
- containedInkWell = false
  - <img src="https://tva1.sinaimg.cn/large/00831rSTgy1gdngo6rauqg308g02amyb.gif" alt="Apr-09-2020 13-40-18" style="zoom:100%;" align="left" />



## W: ListWheelScrollView

<img src="https://tva1.sinaimg.cn/large/00831rSTgy1gdjtudyhg1j30dw0ozt9g.jpg" style="zoom:50%;" />





# W: TextField

https://juejin.im/post/5c4c4e22f265da6174652fb4

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191230202121127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psMTg2MDM1NDM1NzI=,size_16,color_FFFFFF,t_70)

| 参数                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [autocorrect](https://api.flutter.dev/flutter/material/TextField/autocorrect.html) | 是否启用自动校正。默认为 true。                              |
| [buildCounter](https://api.flutter.dev/flutter/material/TextField/buildCounter.html) | 生成自定义 InputDecorator.counter 小部件的回调。<br/>![image-20200410133952416](https://tva1.sinaimg.cn/large/00831rSTgy1gdom8j4gydj308401r746.jpg) |
| controller                                                   | 控制正在编辑的文本。如果为空，此小部件将创建自己的 TextEditingController。 |
| cursorColor                                                  | 绘制光标时要使用的颜色。<br/>缺省值为 ThemeData.cursorColor 或 cupertino / otheme.primarycolor 取决于 ThemeData.platform。 |
| cursorRadius                                                 | 光标角的圆角应该是多圆。<br/>默认情况下，光标没有半径。      |
| [cursorWidth](https://api.flutter.dev/flutter/material/TextField/cursorWidth.html) | 光标会有多厚。默认为2.0<br/>光标将在文本下面绘制。 光标的宽度将延伸到从左到右文本字符之间的边界的右侧，并延伸到从右到左文本的左侧。 这对应于相对于所选位置的下游延伸。 负值可以用来逆转这种行为。 |
| [decoration](https://api.flutter.dev/flutter/material/TextField/decoration.html) → [InputDecoration](https://api.flutter.dev/flutter/material/InputDecoration-class.html) | [InputDecoration](https://api.flutter.dev/flutter/material/InputDecoration-class.html)<br/>在文字区周围展示装饰。<br/>默认情况下，在文本字段下面绘制一条水平线，但是可以配置为显示图标、标签、提示文本和错误文本。<br/>指定 null 以完全删除装饰(包括装饰引入的额外填充以节省标签空间)。 |
| [dragStartBehavior](https://api.flutter.dev/flutter/material/TextField/dragStartBehavior.html) |                                                              |
| [enabled](https://api.flutter.dev/flutter/material/TextField/enabled.html) | 如果为 false，则文本字段为“禁用” : 它忽略分级，其装饰为灰色。<br/>如果非空，此属性将覆盖装饰的 decorationenabled 属性。 |
| [enableInteractiveSelection](https://api.flutter.dev/flutter/material/TextField/enableInteractiveSelection.html) | 如果为真，则长按此 TextField 将选择文本并显示剪切 / 复制 / 粘贴菜单，点击将移动文本插入符号。<br/>默认情况下为 True。<br/> |
| [enableSuggestions](https://api.flutter.dev/flutter/material/TextField/enableSuggestions.html) |                                                              |
| [expands](https://api.flutter.dev/flutter/material/TextField/expands.html) | 此小部件的高度是否调整为填充其父部件的大小。<br/>如果设置为 true 并包装在 Expanded 或 SizedBox 等父小部件中，则输入将展开以填充父小部件。<br/>当设置为 true 时，maxLines 和 minLines 都必须为 null，否则将抛出错误。<br/>默认为 false。 |
| [focusNode](https://api.flutter.dev/flutter/material/TextField/focusNode.html) |                                                              |
| [inputFormatters](https://api.flutter.dev/flutter/material/TextField/inputFormatters.html) | 可选的输入验证和格式重写。<br/>Formatters are run in the provided order when the text input changes. |
| [keyboardAppearance](https://api.flutter.dev/flutter/material/TextField/keyboardAppearance.html) | 这个设置只能在 iOS 设备上使用。<br/>keyboardAppearance: Brightness.light<br/>keyboardAppearance: Brightness.dark<br/>如果取消设置，默认为 ThemeData.primaryColorBrightness 的亮度。 |
| [keyboardType](https://api.flutter.dev/flutter/material/TextField/keyboardType.html) | 用于编辑文本的键盘类型。<br/>![image-20200410141124171](https://tva1.sinaimg.cn/large/00831rSTgy1gdon5cj5iwj307h065glt.jpg) |
| [maxLength](https://api.flutter.dev/flutter/material/TextField/maxLength.html) | 文本字段中允许的最大字符数(Unicode 标量值)。                 |
| [maxLengthEnforced](https://api.flutter.dev/flutter/material/TextField/maxLengthEnforced.html) | 如果设置了 maxLength，maxLengthEnforced 将指示是否强制该限制 |
| [maxLines](https://api.flutter.dev/flutter/material/TextField/maxLines.html) |                                                              |
| [minLines](https://api.flutter.dev/flutter/material/TextField/minLines.html) |                                                              |
| [obscureText](https://api.flutter.dev/flutter/material/TextField/obscureText.html) | 是否隐藏正在编辑的文本(例如，用于密码)。                     |
| [readOnly](https://api.flutter.dev/flutter/material/TextField/readOnly.html) | 设置为 true 时，文本不能通过任何快捷键或键盘操作进行修改。 文本仍然是可选择的。 |
| [scrollController](https://api.flutter.dev/flutter/material/TextField/scrollController.html) | The [ScrollController](https://api.flutter.dev/flutter/widgets/ScrollController-class.html) to use when vertically scrolling the input. |
| [scrollPadding](https://api.flutter.dev/flutter/material/TextField/scrollPadding.html) |                                                              |
| [selectionEnabled](https://api.flutter.dev/flutter/material/TextField/selectionEnabled.html) |                                                              |
| [showCursor](https://api.flutter.dev/flutter/material/TextField/showCursor.html) |                                                              |
| [strutStyle](https://api.flutter.dev/flutter/material/TextField/strutStyle.html) |                                                              |
| [style](https://api.flutter.dev/flutter/material/TextField/style.html) → [TextStyle](https://api.flutter.dev/flutter/painting/TextStyle-class.html) | The style to use for the text being edited.                  |
| [textAlign](https://api.flutter.dev/flutter/material/TextField/textAlign.html) |                                                              |
| [textAlignVertical](https://api.flutter.dev/flutter/material/TextField/textAlignVertical.html) | null                                                         |
| [textCapitalization](https://api.flutter.dev/flutter/material/TextField/textCapitalization.html) |                                                              |
| [textCapitalization](https://api.flutter.dev/flutter/material/TextField/textCapitalization.html) | 配置平台键盘选择大写或小写键盘的方式。                       |
| [textInputAction](https://api.flutter.dev/flutter/material/TextField/textInputAction.html) | 用于键盘的操作按钮的类型。                                   |
| [toolbarOptions](https://api.flutter.dev/flutter/material/TextField/toolbarOptions.html) → [ToolbarOptions](https://api.flutter.dev/flutter/widgets/ToolbarOptions-class.html) | ![image-20200410143109657](https://tva1.sinaimg.cn/large/00831rSTgy1gdonpwsw2hj303v02ijrb.jpg) |



## 事件回调

| 事件                                                         |      |
| ------------------------------------------------------------ | ---- |
| [onTap](https://api.flutter.dev/flutter/material/TextField/onTap.html) |      |
| [onChanged](https://api.flutter.dev/flutter/material/TextField/onChanged.html) |      |
| [onEditingComplete](https://api.flutter.dev/flutter/material/TextField/onEditingComplete.html) |      |
| [onSubmitted](https://api.flutter.dev/flutter/material/TextField/onSubmitted.html) |      |



# GestureDetector

*检测手势*

```dart
behavior: HitTestBehavior.opaque,
```



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
