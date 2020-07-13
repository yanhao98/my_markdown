# 计算文字宽高

```dart
double paintWidthWithTextStyle(TextStyle style) {
      final TextPainter textPainter = TextPainter(text: TextSpan(text: 'Message', style: style), maxLines: 1, textDirection: TextDirection.ltr)..layout(minWidth: 0, maxWidth: double.infinity);
      return textPainter.size.height;
    }
```



# 字重对应

100 - Thin
200 - Extra Light (Ultra Light)
300 - Light
400 - Normal
500 - Medium
600 - Semi Bold (Demi Bold)
700 - Bold
800 - Extra Bold (Ultra Bold)
900 - Black (Heavy)