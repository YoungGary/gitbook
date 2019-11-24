# Flutter 起步

## Flutter 环境搭建

这一部分可以查看[官方文档](https://flutterchina.club/get-started/install/)

## Flutter 目录结构介绍

当你搭建好环境后 可以在一个目录的命令行中输入
```
flutter create +项目名
```
创建一个flutter 项目，当创建成功后使用vscode打开当前的文件夹，大概是如下的结构。
![1.png](https://i.loli.net/2019/11/23/GQhnskFaScD9fgC.png)

我们着重需要注意一下几个文件夹

|  文件夹   | 作用  |
|  ----  | ----  |
| android  | android 平台相关代码 |
| ios  | flutter 相关代码， 我们主要编写的代码就在这个文件夹 |
| lib  | ios 平台相关代码 |
| test  | 用于存放测试代码 |
| pubspec.yaml  | 配置文件， 一般存放一些第三方库的依赖。 |

## Flutter 入口文件、 入口方法

每一个 flutter 项目的 lib 目录里面都有一个 main.dart 这个文件就是 flutter 的入口文件

在main.dart中
``` dart
void main(){
runApp(MyApp());
}
//也可以简写
void main()=>runApp(MyApp());
```
其中的 main 方法是 dart 的入口方法。 runApp 方法是 flutter 的入口方法。
MyApp 是自定义的一个组件

## Flutter 第一个组件
上文提到在runApp方法中 MyApp是一个组件 那么我们就开始定义这个组件

在 Flutter 中自定义组件其实就是一个类， 这个类需要继承 StatelessWidget/StatefulWidget

StatelessWidget 是无状态组件， 状态不可变的 widget
StatefulWidget 是有状态组件， 持有的状态可能在 widget 生命周期改变

``` dart
mport 'package:flutter/material.dart';
void main(){
    runApp(MyApp());
} 
class MyApp extends StatelessWidget{
@override
Widget build(BuildContext context) {
    // TODO: implement build
    return Center(
        child: Text(
            "我是一个文本内容",
                textDirection:TextDirection.ltr,
            ),
        );
    }
}
```
在MyApp 这个类中会重写build 方法返回这个组件。 在Flutter 中 Center 组件表示这个组件是居中显示的 在child中包裹一个text组件 并显示文字。 Text 也有textDirection属性，可以设置文字的对齐方式。 这就是声明式UI 的写法。关于Center，Text组件的用法 会在后面详细讲。

## 给Text 组件增加一些装饰属性
``` dart
import 'package:flutter/material.dart';
void main(){
  runApp(MyApp());
} 
class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
  // TODO: implement build
    return Center(
        child: Text(
            "我是 Dart 一个文本内容",
            textDirection: TextDirection.ltr,
            style: TextStyle(
                fontSize: 40.0,
                fontWeight: FontWeight.bold,
                // color: Colors.yellow
                color: Color.fromRGBO(255, 222, 222, 0.5)
            ),
        ),
        );
    }
}
```

##  使用 MaterialApp 和 Scaffold两个组件装饰 App

### MaterialApp
MaterialApp 是一个方便的 Widget，它封装了应用程序实现 Material Design 所需要的
一些 Widget。一般作为顶层 widget 使用。
常用的属性：
home（ 主页）
title（ 标题）
color（ 颜色）
theme（ 主题）
routes（ 路由）

### Scaffold
Scaffold 是 Material Design 布局结构的基本实现。此类提供了用于显示 drawer、
snackbar 和底部 sheet 的 API。

caffold 有下面几个主要属性：

appBar - 显示在界面顶部的一个 AppBar。

body - 当前界面所显示的主要内容 Widget。

drawer - 抽屉菜单控件。

``` dart
import 'package:flutter/material.dart';
void main(){
    runApp(MyApp());
} 
class MyApp extends StatelessWidget{
    @override
    Widget build(BuildContext context) {
        // TODO: implement build
        return MaterialApp(
            title:"我是一个标题",
            home:Scaffold(
                appBar: AppBar(
                    title:Text('data'),
                    elevation: 30.0, //设置标题阴影 不需要的话值设置成 0.0
                )
                body: MyHome(),
            ),
            theme: ThemeData( //设置主题颜色
                primarySwatch: Colors.yellow
            ),
        );
    }
} 

class MyHome extends StatelessWidget{
    @override
    Widget build(BuildContext context) {
        // TODO: implement build
        return Center(
            child: Text(
                "我是 Dart 一个文本内容",
                textDirection: TextDirection.ltr,
                style: TextStyle(
                    fontSize: 40.0,
                    fontWeight: FontWeight.bold,
                    color: Colors.black38
                    // color: Color.fromRGBO(255, 222, 222, 0.5)
                ),
            ),
        );
    }
}
```

## Vscode 调试 Flutter 项目

Vscode 中打开 flutter 项目进行开发

运行 Flutter 项目
```
flutter run
```
r 键： 点击后热加载， 也就算是重新加载吧。

p 键： 显示网格， 这个可以很好的掌握布局情况， 工作中很有用。

o 键： 切换 android 和 ios 的预览模式。

q 键： 退出调试预览模式。