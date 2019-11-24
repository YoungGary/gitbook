# Container 组件
 我们使用上一章的创建出来的MaterialApp 来创建一个基本的模板
 在body 中 创建一个ContainerWidget

 ``` dart
 import 'package:flutter/material.dart';

// void main() => runApp(HelloWorld());
void main() => runApp(Mertial());

// Material App widget
class Mertial extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
          appBar: AppBar(
            title: Text('hello flutter'),
          ),
          // 
          body:ContainerWidget(),
        ),
    );
  }
}
 ```
 我们创建这个widget 在build 方法中rerurn Container组件
 ``` dart
 // container
// 容器组件
//child
// width 
//height
// decoration

class ContainerWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text('1234567890123456789',
          style: TextStyle(
            color: Colors.red,
            fontSize: 40
          ),
          textAlign: TextAlign.center,
          overflow: TextOverflow.ellipsis,
          maxLines:2,
          textScaleFactor: 2,
      ),
      width:200,
      height: 300, 
      decoration: BoxDecoration(
        color: Colors.yellow,
        border: Border.all(
          color: Colors.blue,
          width: 2
        )
      ), 
    );
  }
}
 ```
 我们Container 组件中有一些属性

|  名称   | 功能  |
|  ----  | ----  |
| alignment  | 对齐方式 |
| decoration  | 装饰属性   |
| margin  | 外边距 EdgeInsets.all(20.0), |
| padding  | 内边距 padding: EdgeInsets.all(10.0) |
| transform  | 让 Container 容易进行一些旋转之类的 |
| height  | 容器高度 |
| width  | 容器宽度 |
| child  | 容器子元素 |

关于Container的更多属性 可以查看官方文档

# Text 组件

刚刚的代码中我们在Container的child属性里 创建了一个Text组件
text组件的第一个属性是text的文字内容 还有一些装饰属性
|  名称   | 功能  |
|  ----  | ----  |
| textAlign  | 文本对齐方式（ center 居中， left 左
对齐， right 右对齐， justfy 两端对齐） |
| textDirection  | 文本方向（ ltr 从左至右， rtl 从右至
左）   |
| overflow  | 文字超出屏幕之后的处理方式（ clip
裁剪， fade 渐隐， ellipsis 省略号） |
| textScaleFactor  | 字体显示倍率 |
| maxLines  | 文字显示最大行数 |
| style  | 字体的样式设置 使用TextStyle 类 |

TextStyle 的参数 ：
|  名称   | 功能  |
|  ----  | ----  |
| decoration  | 文字装饰线（ none 没有线， lineThrough 删
除线， overline 上划线， underline 下划线） |
| decorationColor  | 文字装饰线颜色   |
| decorationStyle  | 文字装饰线风格（ dashed,dotted，double 两根线， solid 一根实线， wavy 波浪线） |
| wordSpacing  | 单词间隙（ 如果是负值， 会让单词变得更紧凑 |
| letterSpacing  | 字母间隙（ 如果是负值， 会让字母变得更凑） |
| fontStyle  | 文字样式（ italic 斜体， normal 正常体） |
| fontSize  | 文字大小 |
| color  | 文字颜色 |
| fontWeight  | 字体粗细（ bold 粗体， normal 正常体） |


