
# Flutter 技术总结分享

![11525642-20add9a2fdd2861d.jpg](https://i.loli.net/2019/11/24/A5aYkLwgRrEZlMV.jpg)

## 前言

 结合[官方文档](https://flutterchina.club/get-started/install/)和《Fluuter技术与入门实践》书籍 详细阐述Flutter技术，涉及Dart语言，Flutter Widget，Flutter状态管理等。

 ## Flutter 是什么

 >Flutter is Google’s UI toolkit for building beautiful, natively compiled applications for mobile, web, and desktop from a single codebase.

Flutter是由原 Google Chrome 团队成员，利用 Chrome 2D 渲染引擎，然后精简 CSS 布局演变而来。架构图如下图
![1.jpg](https://i.loli.net/2019/11/17/AnBwTMHOFsdvREx.png)
* Flutter 在各个原生的平台中，使用自己的 C++的引擎渲染界面，没有使用 webview，也不像 RN、NativeScript 一样使用系统的组件。简单来说平台只是给 Flutter 提供一个画布。
* 界面使用 Dart 语言开发，貌似唯一支持 JIT（Just-In-Time），和 AOT （Ahead-Of-Time）模式的强类型语言。
* 写法非常的现代，声明式，组件化，Composition > inheritance，响应式……就是现在前端流行的这一套
* 一套代码搞定所有平台。

## 移动开发技术简介

先主要介绍一下移动开发技术的进化历程，主要是想让读者知道Flutter技术出现的背景。笔者认为，了解一门新技术出现的背景是非常重要的，因为只有了解之前是什么样的，才能理解为什么会是现在这样。

### 原生开发与跨平台技术

原生开发

原生应用程序是指某一个移动平台（比如iOS或安卓）所特有的应用，使用相应平台支持的开发工具和语言，并直接调用系统提供的SDK API。比如Android原生应用就是指使用Java或Kotlin语言直接调用Android SDK开发的应用程序；而iOS原生应用就是指通过Objective-C或Swift语言直接调用iOS SDK开发的应用程序。原生开发有以下主要优势：
* 可访问平台全部功能（GPS、摄像头）；
* 速度快、性能高、可以实现复杂动画及绘制，整体用户体验好；

主要缺点：

* 平台特定，开发成本高；不同平台必须维护不同代码，人力成本随之变大；
* 内容固定，动态化弱，大多数情况下，有新功能更新时只能发版；

在移动互联网发展初期，业务场景并不复杂，原生开发还可以应对产品需求迭代。 但近几年，随着物联网时代到来、移动互联网高歌猛进，日新月异，在很多业务场景中，传统的纯原生开发已经不能满足日益增长的业务需求。主要表现在：

* 动态化内容需求增大；当需求发生变化时，纯原生应用需要通过版本升级来更新内容，但应用上架、审核是需要周期的，这对高速变化的互联网时代来说是很难接受的，所以，对应用动态化(不发版也可以更新应用内容)的需求就变的迫在眉睫。
* 业务需求变化快，开发成本变大；由于原生开发一般都要维护Android、iOS两个开发团队，版本迭代时，无论人力成本，还是测试成本都会变大。

总结一下，纯原生开发主要面临动态化和开发成本两个问题，而针对这两个问题，诞生了一些跨平台的动态化框架。

### 跨平台技术简介

针对原生开发面临问题，人们一直都在努力寻找好的解决方案，而时至今日，已经有很多跨平台框架(注意，本书中所指的“跨平台”若无特殊说明，即特指Android和iOS两个平台)，根据其原理，主要分为三类：

* H5+原生（Cordova、Ionic、微信小程序）
* JavaScript开发+原生渲染 （React Native、Weex、快应用）
* 自绘UI+原生(QT for mobile、Flutter)

#### H5+原生混合开发

这类框架主要原理就是将APP的一部分需要动态变动的内容通过H5来实现，通过原生的网页加载控件WebView (Android)或WKWebView（iOS）来加载（以后若无特殊说明，我们用WebView来统一指代android和iOS中的网页加载控件）。这样以来，H5部分是可以随时改变而不用发版，动态化需求能满足；同时，由于h5代码只需要一次开发，就能同时在Android和iOS两个平台运行，这也可以减小开发成本，也就是说，H5部分功能越多，开发成本就越小。我们称这种h5+原生的开发模式为混合开发 ，采用混合模式开发的APP我们称之为混合应用或Hybrid APP ，如果一个应用的大多数功能都是H5实现的话，我们称其为Web APP 。

目前混合开发框架的典型代表有：Cordova、Ionic 和微信小程序，值得一提的是微信小程序目前是在webview中渲染的，并非原生渲染，但将来有可能会采用原生渲染。

原理

如之前所述，原生开发可以访问平台所有功能，而混合开发中，H5代码是运行在WebView中，而WebView实质上就是一个浏览器内核，其JavaScript依然运行在一个权限受限的沙箱中，所以对于大多数系统能力都没有访问权限，如无法访问文件系统、不能使用蓝牙等。所以，对于H5不能实现的功能，都需要原生去做。而混合框架一般都会在原生代码中预先实现一些访问系统能力的API， 然后暴露给WebView以供JavaScript调用，这样一来，WebView就成为了JavaScript与原生API之间通信的桥梁，主要负责JavaScript与原生之间传递调用消息，而消息的传递必须遵守一个标准的协议，它规定了消息的格式与含义，我们把依赖于WebView的用于在JavaScript与原生之间通信并实现了某种消息传输协议的工具称之为WebView JavaScript Bridge, 简称 JsBridge，它也是混合开发框架的核心。

>示例：JavaScript调用原生API获取手机型号

我们以Android为例，实现一个获取手机型号的原生API供JavaScript调用。在这个示例中将展示JavaScript调用原生API的流程，可以直观的感受一下调用流程。我们选用在Github上开源的dsBridge作为JsBridge来进行通信。dsBridge是一个支持同步调用的跨平台的JsBridge，此示例中只使用其同步调用功能。

* 首先在原生中实现获取手机型号的API getPhoneModel
``` java
class JSAPI {

 @JavascriptInterface
 public Object getPhoneModel(Object msg) {

   return Build.MODEL;

 }
}
```
* 将原生API通过WebView注册到JsBridge中
``` java
import wendu.dsbridge.DWebView
...
//DWebView继承自WebView，由dsBridge提供  
DWebView dwebView = (DWebView) findViewById(R.id.dwebview);
//注册原生API到JsBridge
dwebView.addJavascriptObject(new JsAPI(), null);
```
* 在JavaScript中调用原生API
``` javascript
var dsBridge = require("dsbridge")
//直接调用原生API `getPhoneModel`
var model = dsBridge.call("getPhoneModel");
//打印机型
console.log(model);
```
上面示例演示了JavaScript调用原生API的过程，同样的，一般来说优秀的JsBridge也支持原生调用JavaScript，dsBridge也是支持的，如果您感兴趣，可以去github dsBridge项目主页查看。

现在，我们回头来看一下，混合应用无非就是在第一步中预先实现一系列API供JavaScript调用，让JavaScript有访问系统的能力，看到这里，我相信你也可以自己实现一个混合开发框架了。

总结

混合应用的优点是动态内容是H5，web技术栈，社区及资源丰富，缺点是性能不好，对于复杂用户界面或动画，WebView不堪重任。

#### JavaScript开发+原生渲染

主要介绍一下 JavaScript开发+原生渲染的跨平台框架原理。

React Native (简称RN)是Facebook于2015年4月开源的跨平台移动应用开发框架，是Facebook早先开源的JS框架 React 在原生移动应用平台的衍生产物，目前支持iOS和Android两个平台。RN使用Javascript语言，类似于HTML的JSX，以及CSS来开发移动应用，因此熟悉Web前端开发的技术人员只需很少的学习就可以进入移动应用开发领域。

由于RN和React原理相通，并且Flutter也是受React启发，很多思想也都是相通的，万丈高楼平地起，我们有必要深入了解一下React原理。React是一个响应式的Web框架，我们先了解一下两个重要的概念：DOM树与响应式编程。

#### DOM树与控件树

文档对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展标志语言的标准编程接口，一种独立于平台和语言的方式访问和修改一个文档的内容和结构。换句话说，这是表示和处理一个HTML或XML文档的标准接口。简单来说，DOM就是文档树，与用户界面控件树对应，在前端开发中通常指HTML对应的渲染树，但广义的DOM也可以指Android中的XML布局文件对应的控件树，而术语DOM操作就是指直接来操作渲染树（或控件树）， 因此，可以看到其实DOM树和控件树是等价的概念，只不过前者常用于Web开发中，而后者常用于原生开发中。

#### 响应式编程

React中提出一个重要思想：状态改变则UI随之自动改变，而React框架本身就是响应用户状态改变的事件而执行重新构建用户界面的工作，这就是典型的响应式编程范式，下面我们总结一下React中响应式原理：

* 开发者只需关注状态转移（数据），当状态发生变化，React框架会自动根据新的状态重新构建UI。
* React框架在接收到用户状态改变通知后，会根据当前渲染树，结合最新的状态改变，通过Diff算法，计算出树中变化的部分，然后只更新变化的部分（DOM操作），从而避免整棵树重构，提高性能。

值得注意的是，在第二步中，状态变化后React框架并不会立即去计算并渲染DOM树的变化部分，相反，React会在DOM的基础上建立一个抽象层，即虚拟DOM树，对数据和状态所做的任何改动，都会被自动且高效的同步到虚拟DOM，最后再批量同步到真实DOM中，而不是每次改变都去操作一下DOM。为什么不能每次改变都直接去操作DOM树？这是因为在浏览器中每一次DOM操作都有可能引起浏览器的重绘或回流：

* 如果DOM只是外观风格发生变化，如颜色变化，会导致浏览器重绘界面。
* 如果DOM树的结构发生变化，如尺寸、布局、节点隐藏等导致，浏览器就需要回流（及重新排版布局）。

而浏览器的重绘和回流都是比较昂贵的操作，如果每一次改变都直接对DOM进行操作，这会带来性能问题，而批量操作只会触发一次DOM更新。

#### React Native

上文已经提到React Native 是React 在原生移动应用平台的衍生产物，那两者主要的区别是什么呢？其实，主要的区别在于虚拟DOM映射的对象是什么？React中虚拟DOM最终会映射为浏览器DOM树，而RN中虚拟DOM会通过 JavaScriptCore 映射为原生控件树。

JavaScriptCore 是一个JavaScript解释器，它在React Native中主要有两个作用：

* 为JavaScript提供运行环境。
* 是JavaScript与原生应用之间通信的桥梁，作用和JsBridge一样，事实上，在iOS中，很多JsBridge的实现都是基于 JavaScriptCore 。

而RN中将虚拟DOM映射为原生控件的过程中分两步：

* 布局消息传递； 将虚拟DOM布局信息传递给原生；
* 原生根据布局信息通过对应的原生控件渲染控件树；

至此，React Native 便实现了跨平台。 相对于混合应用，由于React Native是原生控件渲染，所以性能会比混合应用中H5好很多，同时React Native使用了Web开发技术栈，也只需维护一份代码，同样是跨平台框架。

#### 总结

JavaScript开发+原生渲染的方式主要优点如下：

* 采用Web开发技术栈，社区庞大、上手快、开发成本相对较低。
* 原生渲染，性能相比H5提高很多。
* 动态化较好，支持热更新。

不足：

* 渲染时需要JavaScript和原生之间通信，在有些场景如拖动可能会因为通信频繁导致卡顿。
* JavaScript为脚本语言，执行时需要JIT(Just In Time)，执行效率和AOT(Ahead Of Time)代码仍有差距。
* 由于渲染依赖原生控件，不同平台的控件需要单独维护，并且当系统更新时，社区控件可能会滞后；除此之外，其控件系统也会受到原生UI系统限制，例如，在Android中，手势冲突消歧规则是固定的，这在使用不同人写的控件嵌套时，手势冲突问题将会变得非常棘手。

## Flutter 为什么快？Flutter 相比 RN 的优势在哪里？
从架构中实际上已经能看出 Flutter 为什么快，至少相比之前的当红炸子鸡 React Native 快的原因了。

* Skia 引擎，Chrome， Chrome OS，Android，Firefox，Firefox OS 都以此作为渲染引擎。
* Dart 语言可以 AOT 编译成 ARM Code，让布局以及业务代码运行的最快，而且 Dart 的 GC 针对 Flutter 频繁销毁创建 Widget 做了专门的优化。
* CSS 的子集 Flex like 的布局方式，保留强大表现能力的同时，也保留了性能。
* Flutter 业务书写的 Widget 在渲染之前 diff 转化成 Render Object，对，就像 React 中的 Virtual DOM，以此来确保开发体验和性能。

而相比 React Native：

* RN 使用 JavaScript 来运行业务代码，然后 JS Bridge 的方式调用平台相关组件，性能比有损失，甚至平台不同 js 引擎都不一样。
* RN 使用平台组件，行为一致性会有打折，或者说，开发者需要处理更多平台相关的问题。

阿里巴巴旗下闲鱼技术团队是国内比较早使用Flutter的团队，他们对于Flutter技术写了很多干货文章，地址在这里 [链接](https://www.yuque.com/xytech/flutter),关于Flutter和RN的性能测试，他们也写了一篇技术文章，可以看[这里](https://www.yuque.com/xytech/flutter/gs3pnk)，结论是 Flutter，在 CPU，FPS，内存稳定上均优于 ReactNative。

## Dart 语言

在开始 Flutter 之前，我们需要先了解下 Dart 语言

Dart 是由 Google 开发，最初是想作为 JavaScript 替代语言，但是失败沉寂之后，作为 Flutter 独有开发语言又焕发了第二春。

实际上即使到了 2.0，Dart 语法和 JavaScriptFlutter非常的相像。单线程，Event Loop……
![2.jpg](https://i.loli.net/2019/11/17/3KtvxBMbcFuaYsU.png)
Dart 相比于Flutter  有一些更甜的语法糖
* 不会飘的 this
* 强类型，当然前端现在有了 TypeScript 
* 强大方便的操作符号：

* <kbd>?.</kbd> 方便安全的 <kbd>foo?.bar</kbd>取值，如果 foo 为 null，那么取值为 null
* <kbd>??</kbd> <kbd>condition?expr1:expr2</kbd> 可以简写为 <kbd>expr1??expr2</kbd>
* <kbd>=</kbd>和其他符号的组合: <kbd>*=</kbd>、 <kbd>~/=</kbd>、 <kbd>&=</kbd>、 <kbd>|=</kbd> ……
* 级联操作符(Cascade notation ..)

比如

``` dart
querySelect('#button')
 ..text ="Confirm"
 ..classes.add('important')
 ..onClick.listen((e) => window.alert('Confirmed'))
```

甚至可以重写操作符

```dart
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  // Operator == and hashCode not shown. For details, see note below.
  // ···
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}


```

注：重写 ==，也需要重写 Object hashCodegetter

``` dart
class Person {
  final String firstName, lastName;

  Person(this.firstName, this.lastName);

  // Override hashCode using strategy from Effective Java,
  // Chapter 11.
  @override
  int get hashCode {
    int result = 17;
    result = 37 * result + firstName.hashCode;
    result = 37 * result + lastName.hashCode;
    return result;
  }

  // You should generally implement operator == if you
  // override hashCode.
  @override
  bool operator ==(dynamic other) {
    if (other is! Person) return false;
    Person person = other;
    return (person.firstName == firstName &&
        person.lastName == lastName);
  }
}

void main() {
  var p1 = Person('Bob', 'Smith');
  var p2 = Person('Bob', 'Smith');
  var p3 = 'not a person';
  assert(p1.hashCode == p2.hashCode);
  assert(p1 == p2);
  assert(p1 != p3);
}
```
这点在 diff 对象的时候尤其有用。

## dart中的异步支持

Dart类库有非常多的返回Future或者Stream对象的函数。 这些函数被称为异步函数：它们只会在设置好一些耗时操作之后返回，比如像 IO操作。而不是等到这个操作完成。

async和await关键词支持了异步编程，允许您写出和同步代码很像的异步代码。

### Future

Future与JavaScript中的Promise非常相似，表示一个异步操作的最终完成（或失败）及其结果值的表示。简单来说，它就是用于处理异步操作的，异步处理成功了就执行成功的操作，异步处理失败了就捕获错误或者停止后续操作。一个Future只会对应一个结果，要么成功，要么失败。

由于本身功能较多，这里我们只介绍其常用的API及特性。还有，请记住，Future 的所有API的返回值仍然是一个Future对象，所以可以很方便的进行链式调用。

### Future.then

为了方便示例，在本例中我们使用Future.delayed 创建了一个延时任务（实际场景会是一个真正的耗时任务，比如一次网络请求），即2秒后返回结果字符串"hi world!"，然后我们在then中接收异步结果并打印结果，代码如下：
``` dart
Future.delayed(new Duration(seconds: 2),(){
   return "hi world!";
}).then((data){
   print(data);
});
```
### Future.catchError
如果异步任务发生错误，我们可以在catchError中捕获错误，我们将上面示例改为：
``` dart
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");  
}).then((data){
   //执行成功会走到这里  
   print("success");
}).catchError((e){
   //执行失败会走到这里  
   print(e);
});
```
在本示例中，我们在异步任务中抛出了一个异常，then的回调函数将不会被执行，取而代之的是 catchError回调函数将被调用；但是，并不是只有 catchError回调才能捕获错误，then方法还有一个可选参数onError，我们也可以它来捕获异常：
``` dart
Future.delayed(new Duration(seconds: 2), () {
    //return "hi world!";
    throw AssertionError("Error");
}).then((data) {
    print("success");
}, onError: (e) {
    print(e);
});
```
### Future.whenComplete
有些时候，我们会遇到无论异步任务执行成功或失败都需要做一些事的场景，比如在网络请求前弹出加载对话框，在请求结束后关闭对话框。这种场景，有两种方法，第一种是分别在then或catch中关闭一下对话框，第二种就是使用Future的whenComplete回调，我们将上面示例改一下：

``` dart
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");
}).then((data){
   //执行成功会走到这里 
   print(data);
}).catchError((e){
   //执行失败会走到这里   
   print(e);
}).whenComplete((){
   //无论成功或失败都会走到这里
});
```

### Future.wait
有些时候，我们需要等待多个异步任务都执行结束后才进行一些操作，比如我们有一个界面，需要先分别从两个网络接口获取数据，获取成功后，我们需要将两个接口数据进行特定的处理后再显示到UI界面上，应该怎么做？答案是Future.wait，它接受一个Future数组参数，只有数组中所有Future都执行成功后，才会触发then的成功回调，只要有一个Future执行失败，就会触发错误回调。下面，我们通过模拟Future.delayed 来模拟两个数据获取的异步任务，等两个异步任务都执行成功时，将两个异步任务的结果拼接打印出来，代码如下：
``` dart
Future.wait([
  // 2秒后返回结果  
  Future.delayed(new Duration(seconds: 2), () {
    return "hello";
  }),
  // 4秒后返回结果  
  Future.delayed(new Duration(seconds: 4), () {
    return " world";
  })
]).then((results){
  print(results[0]+results[1]);
}).catchError((e){
  print(e);
});
```
执行上面代码，4秒后你会在控制台中看到“hello world”。

<!-- ### lsolate

Dart 运行在独立隔离的 iSolate 中就类似 JavaScript 一样，单线程事件驱动，但是 Dart 也开放了创建其他 isolate，充分利用 CPU 的多和能力。

``` dart
loadData() async {
   // 通过spawn新建一个isolate，并绑定静态方法
   ReceivePort receivePort =ReceivePort();
   await Isolate.spawn(dataLoader, receivePort.sendPort);

   // 获取新isolate的监听port
   SendPort sendPort = await receivePort.first;
   // 调用sendReceive自定义方法
   List dataList = await sendReceive(sendPort, 'https://hicc.me/posts');
   print('dataList $dataList');
}

// isolate的绑定方法
static dataLoader(SendPort sendPort) async{
   // 创建监听port，并将sendPort传给外界用来调用
   ReceivePort receivePort =ReceivePort();
   sendPort.send(receivePort.sendPort);

   // 监听外界调用
   await for (var msg in receivePort) {
     String requestURL =msg[0];
     SendPort callbackPort =msg[1];

     Client client = Client();
     Response response = await client.get(requestURL);
     List dataList = json.decode(response.body);
     // 回调返回值给调用者
     callbackPort.send(dataList);
  }    
}

// 创建自己的监听port，并且向新isolate发送消息
Future sendReceive(SendPort sendPort, String url) {
   ReceivePort receivePort =ReceivePort();
   sendPort.send([url, receivePort.sendPort]);
   // 接收到返回值，返回给调用者
   return receivePort.first;
}


```

当然 Flutter 中封装了[compute](https://api.flutter.dev/flutter/foundation/compute.html)，可以方便的使用，譬如[在其它 isolate 中解析大的 json](https://flutter.dev/docs/cookbook/networking/background-parsing)。 -->

## Dart UI as Code

在这里单独提出来的意义在于，从 React 开始，到 Flutter，到最近的 Apple SwiftUI，Android Jetpack Compose 声明式组件写法越发流行，Web 前端使用 JSX 来让开发者更方便的书写，而 Flutter，SwiftUI 则直接从优化语言本身着手。

**函数类的命名参数**

``` dart
void test({@required int age,String name}) {
  print(name);
  print(age);
}
// 解决函数调用时候，参数不明确的问题
test(name:"hicc",age: 30)

// 这样对于组件的使用尤为方便
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
  return Scaffold(
      appBar: AppBar(),
      body: Container(),
      floatingActionButton:FloatingActionButton()
    );
  }
}
```

大杀器：Collection If 和 Collection For

``` dart
// collection If
Widget build(BuildContext context) {
  return Row(
    children: [
      IconButton(icon: Icon(Icons.menu)),
      Expanded(child: title),
      if (!isAndroid)
        IconButton(icon: Icon(Icons.search)),
    ],
  );
}
```

``` dart
// Collect For
var command = [
  engineDartPath,
  frontendServer,
  for (var root in fileSystemRoots) "--filesystem-root=$root",
  for (var entryPoint in entryPoints)
    if (fileExists("lib/$entryPoint.json")) "lib/$entryPoint",
  mainPath
];

```

## Flutter 怎么写

到这里终于到正题了，熟悉 React 的话，你会发现异常的熟悉。<kbd> UI=F(state)</kbd>
![3.jpg](https://i.loli.net/2019/11/17/g9ke3ARsl2oj7rp.png)
Flutter App 的一切从 <kbd>lib/main.dart</kbd>文件的 main 函数开始：

``` dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}

```

Dart 类 build 方法返回的便是 Widget，在 Flutter 中一切都是 Widget，包括但不限于
* 结构性元素，menu，button 等
* 样式类元素，font，color 等
* 布局类元素，padding，margin 等
* 导航
* 手势

Widget 是 Dart 中特殊的类，通过实例化(Dart 中new 是可选的)相互嵌套，你的这个 App 就是形如下图的一颗组件树(Dart 入口函数的概念， main.dart->main())。
![4.jpg](https://i.loli.net/2019/11/17/VO4inQazkTNhlEu.png)

### Widget 布局

>Flutter中万物皆Widget

最开始说过 Flutter 布局思路来自 CSS，而 Flutter 中一切皆 Widget，因此整体布局也很简单：

* 容器组件 Container
* - decoration 装饰属性，设置背景色，背景图，边框，圆角，阴影和渐变等
  - margin
  - padding
  - alignment
  - width
  - height
* Padding，Center
* Row,Column,Flex
* Wrap, Flow 流式布局
* stack， z 轴布局
* 。。。。。。

关于widget 更多可以查看咸鱼团队的[文章](https://www.yuque.com/xytech/flutter/hc0xq7)

Flutter 中 Widget 可以分为三类，形如 React 中“展示组件”、“容器组件”，“context”。

### StatelessWidget

这个就是 Flutter 中的“展示组件”，自身不保存状态，外部参数变化就销毁重新创建。Flutter 建议尽量使用无状态的组件。

### StatefulWidget

状态组件就是类似于 React 中的“容器组件”了，Flutter 中状态组件写法会稍微不一样。

``` dart
class Counter extends StatefulWidget {
  // This class is the configuration for the state. It holds the
  // values (in this case nothing) provided by the parent and used by the build
  // method of the State. Fields in a Widget subclass are always marked "final".

  @override
  _CounterState createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      // This call to setState tells the Flutter framework that
      // something has changed in this State, which causes it to rerun
      // the build method below so that the display can reflect the
      // updated values. If you change _counter without calling
      // setState(), then the build method won't be called again,
      // and so nothing would appear to happen.
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    // This method is rerun every time setState is called, for instance
    // as done by the _increment method above.
    // The Flutter framework has been optimized to make rerunning
    // build methods fast, so that you can just rebuild anything that
    // needs updating rather than having to individually change
    // instances of widgets.
    return Row(
      children: <Widget>[
        RaisedButton(
          onPressed: _increment,
          child: Text('Increment'),
        ),
        Text('Count: $_counter'),
      ],
    );
  }
}

```

可以看到 Flutter 中直接使用了和 React 中同名的 setState方法，不过不会有变量合并的东西，当然也有[生命周期](https://segmentfault.com/a/1190000015211309)。

![5.jpg](https://i.loli.net/2019/11/17/bYhFOATfLr7eJ4N.png)

可以看到一个有状态的组件需要两个 Class，这样写的原因在于，Flutter 中 Widget 都是 immmutable 的，状态组件的状态保存在 State 中，组件仍然每次重新创建，Widget 在这里只是一种对组件的描述，Flutter 会 diff 转换成 Element，然后转换成 RenderObject 才渲染。

![6.jpg](https://i.loli.net/2019/11/17/s7QS5zZN92LrPfV.jpg)

Flutter Widget 更多的渲染流程可以看[这里](https://www.yuque.com/xytech/flutter/tge705)。

实际上 Widget 只是作为组件结构一种描述，还可以带来的好处是，你可以更方便的做一些主题性的组件, Flutter 官方提供的Material Components widgets和Cupertino (iOS-style) widgets质量就相当高，再配合 Flutter 亚秒级的Hot Reload，开发体验可以说挺不错的。


## State Management

setState()可以很方便的管理组件内的数据，但是 Flutter 中状态同样是从上往下流转的，因此也会遇到和 React 中同样的问题，如果组件树太深，逐层状态创建就显得很麻烦了，更不要说代码的易读和易维护性了。

### InheritedWidget

同样 Flutter 也有个 context一样的东西，那就是 InheritedWidget，使用起来也很简单。

``` dart
class GlobalData extends InheritedWidget {
  final int count;
  GlobalData({Key key, this.count,Widget child}):super(key:key,child:child);

  @override
  bool updateShouldNotify(GlobalData oldWidget) {
    return oldWidget.count != count;
  }

  static GlobalData of(BuildContext context) => context.inheritFromWidgetOfExactType(GlobalData);
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);

  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: GlobalData(
        count: _counter,
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text(
                'You have pushed the button this many times:',
              ),
              Text(
                '$_counter',
                style: Theme.of(context).textTheme.display1,
              ),
              Body(),
              Body2()
            ],
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}

class Body extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    GlobalData globalData = GlobalData.of(context);
    return Text(globalData.count.toString());
  }
}

class Body2 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    GlobalData globalData = GlobalData.of(context);
    return Text(globalData.count.toString());
  }

```

具体实现原理可以参考[这里](https://loveky.github.io/2018/07/18/how-flutter-inheritedwidget-works/)，不过 Google 封装了一个更为上层的库[provider](https://pub.dev/packages/provider)，具体使用可以看[这里](https://flutter.dev/docs/development/data-and-backend/state-mgmt/simple)。

### BlOC

[BlOC](https://link.juejin.im/?target=https%3A%2F%2Fmedium.com%2Fflutterpub%2Farchitecting-your-flutter-project-bd04e144a8f1)是 Flutter team 提出建议的另一种更高级的数据组织方式。简单来说：

#### Bloc = InheritedWidget + RxDart(Stream)

Dart 语言中内置了 Steam，Stream ~= Observable，配合[RxDart](https://pub.dev/packages/rxdart), 然后加上 StreamBuilder会是一种异常强大和自由的模式。

``` dart
class GlobalData extends InheritedWidget {
  final int count;
  final Stream<String> timeInterval$ = new Stream.periodic(Duration(seconds: 10)).map((time) => new DateTime.now().toString());
  GlobalData({Key key, this.count,Widget child}):super(key:key,child:child);

  @override
  bool updateShouldNotify(GlobalData oldWidget) {
    return oldWidget.count != count;
  }

  static GlobalData of(BuildContext context) => context.inheritFromWidgetOfExactType(GlobalData);

}

class TimerView extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    GlobalData globalData = GlobalData.of(context);
    return StreamBuilder(
        stream: globalData.timeInterval$,
        builder: (context, snapshot) {
          return Text(snapshot?.data ?? '');
        }
    );
  }
}

```

当然 Bloc 的问题在于
* 学习成本略高，Rx 的概念要吃透，不然你会抓狂
* 自由带来的问题是，可能代码不如 Redux 类的规整。

今年 Apple 也拥抱了响应式，Combine(Rx like) + SwiftUI 也基本等于 Bloc 了。
所以，Rx 还是要赶紧学起来
除去 Bloc，Flutter 中还是可以使用其他的方案，譬如：

* [Flutter Redux](https://pub.dev/packages/flutter_redux)
* 阿里闲鱼的[Fish Redux](https://github.com/alibaba/fish-redux)，据说性能很好。
* [Mobx](https://mobx.pub/)
* ……

展开来说现在的前端开发使用强大的框架页面组装已经不是难点了。开发的难点在于如何组合富交互所需的数据，也就是上面图中的 state部分。

更具体来说，是怎么优雅，高效，易维护地处理短暂数据(ephemeral state) setState()和需要共享的 App State 的问题，这是个工程性的问题，但往往也是日常开发最难的事情了.

到这里，主要的部分已经总结完了，有这些已经可以开发出一个不错的 App 了。

## 与后端的通信

### 1 通过HttpClient发起HTTP请求

Dart IO库中提供了用于发起Http请求的一些类，我们可以直接使用HttpClient来发起请求。使用HttpClient发起请求分为五步：

* 创建一个HttpClient：

``` dart
 HttpClient httpClient = new HttpClient();
```

* 打开Http连接，设置请求头：

``` dart
HttpClientRequest request = await httpClient.getUrl(uri);
```
这一步可以使用任意Http Method，如httpClient.post(...)、httpClient.delete(...)等。如果包含Query参数，可以在构建uri时添加，如：
``` dart
Uri uri=Uri(scheme: "https", host: "flutterchina.club", queryParameters: {
    "xx":"xx",
    "yy":"dd"
  });
```
通过HttpClientRequest可以设置请求header，如：
``` dart
request.headers.add("user-agent", "test");
```
如果是post或put等可以携带请求体方法，可以通过HttpClientRequest对象发送request body，如：
``` dart
String payload="...";
request.add(utf8.encode(payload)); 
//request.addStream(_inputStream); //可以直接添加输入流
```
* 等待连接服务器：

``` dart
HttpClientResponse response = await request.close();
```
这一步完成后，请求信息就已经发送给服务器了，返回一个HttpClientResponse对象，它包含响应头（header）和响应流(响应体的Stream)，接下来就可以通过读取响应流来获取响应内容。

* 读取响应内容：

``` dart
String responseBody = await response.transform(utf8.decoder).join();
```
我们通过读取响应流来获取服务器返回的数据，在读取时我们可以设置编码格式，这里是utf8。

* 请求结束，关闭HttpClient：

``` dart
httpClient.close();
```
关闭client后，通过该client发起的所有请求都会中止。

### 2.Http请求-Dio http库

通过上一节介绍，我们可以发现直接使用HttpClient发起网络请求是比较麻烦的，很多事情得我们手动处理，如果再涉及到文件上传/下载、Cookie管理等就会非常繁琐。幸运的是，Dart社区有一些第三方http请求库，用它们来发起http请求将会简单的多，本节我们介绍一下目前人气较高的[dio](https://github.com/flutterchina/dio)库。

>dio是一个强大的Dart Http请求库，支持Restful API、FormData、拦截器、请求取消、Cookie管理、文件上传/下载、超时等。dio的使用方式随着其版本升级可能会发生变化，如果本节所述内容和dio官方有差异，请以dio官方文档为准。

#### 引入
引入dio:
``` dart
dependencies:
  dio: ^x.x.x #请使用pub上的最新版本
```
导入并创建dio实例：
``` dart
import 'package:dio/dio.dart';
Dio dio =  Dio();
```
接下来就可以通过 dio实例来发起网络请求了，注意，一个dio实例可以发起多个http请求，一般来说，APP只有一个http数据源时，dio应该使用单例模式。

>GET请求

``` dart
Response response;
response=await dio.get("/test?id=12&name=wendu")
print(response.data.toString());
```
带参数
``` dart
response=await dio.get("/test",queryParameters:{"id":12,"name":"wendu"})
print(response);
```
多个并发请求
``` dart
response= await Future.wait([dio.post("/info"),dio.get("/token")]);
```
下载文件:
``` dart
response=await dio.download("https://www.google.com/",_savePath);
```
发送 FormData:
``` dart
FormData formData = new FormData.from({
   "name": "wendux",
   "age": 25,
});
response = await dio.post("/info", data: formData)
```
案例

我们通过Github开放的API来请求flutterchina组织下的所有公开的开源项目，实现：

* 在请求阶段弹出loading
* 请求结束后，如果请求失败，则展示错误信息；如果成功，则将项目名称列表展示出来。
代码如下：
``` dart
class _FutureBuilderRouteState extends State<FutureBuilderRoute> {
  Dio _dio = new Dio();

  @override
  Widget build(BuildContext context) {

    return new Container(
      alignment: Alignment.center,
      child: FutureBuilder(
          future: _dio.get("https://api.github.com/orgs/flutterchina/repos"),
          builder: (BuildContext context, AsyncSnapshot snapshot) {
            //请求完成
            if (snapshot.connectionState == ConnectionState.done) {
              Response response = snapshot.data;
              //发生错误
              if (snapshot.hasError) {
                return Text(snapshot.error.toString());
              }
              //请求成功，通过项目信息构建用于显示项目名称的ListView
              return ListView(
                children: response.data.map<Widget>((e) =>
                    ListTile(title: Text(e["full_name"]))
                ).toList(),
              );
            }
            //请求未完成时弹出loading
            return CircularProgressIndicator();
          }
      ),
    );
  }
}
```




## 测试

Flutter debugger，测试都是出场自带，用起来也不难。

``` dart
// 测试在/test/目录下面
void main() {

  testWidgets('Counter increments smoke test', (WidgetTester tester) async {
    // Build our app and trigger a frame.
    await tester.pumpWidget(MyApp());

    // Verify that our counter starts at 0.
    expect(find.text('0'), findsOneWidget);
    expect(find.text('1'), findsNothing);

    // Tap the '+' icon and trigger a frame.
    await tester.tap(find.byIcon(Icons.add));
    await tester.pump();

    // Verify that our counter has incremented.
    expect(find.text('0'), findsNothing);
    expect(find.text('1'), findsOneWidget);
  });
}

```

## 包管理，资源管理

类似与 JavaScript 的 npm，Flutter，也就是 Dart 也有自己的包仓库。不过项目包的依赖使用 yaml 文件来描述:

``` dart
name: app
description: A new Flutter project.
version: 1.0.0+1

environment:
  sdk: ">=2.1.0 <3.0.0"

dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^0.1.2

```

## 生命周期

移动应用总归需要应用级别的生命周期，flutter 中使用生命周期钩子，也非常的简单：

``` dart
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => new _MyAppState();
}

class _MyAppState extends State<MyApp> with WidgetsBindingObserver {
  @override
  void initState() {
    super.initState();
    WidgetsBinding.instance.addObserver(this);
  }

  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    super.dispose();
  }

  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    switch (state) {
      case AppLifecycleState.inactive:
        print('AppLifecycleState.inactive');
        break;
      case AppLifecycleState.paused:
        print('AppLifecycleState.paused');
        break;
      case AppLifecycleState.resumed:
        print('AppLifecycleState.resumed');
        break;
      case AppLifecycleState.suspending:
        print('AppLifecycleState.suspending');
        break;
    }
    super.didChangeAppLifecycleState(state);
  }

  @override
  Widget build(BuildContext context) {
      return Container();
  }
}
```
## 使用原生能力

由于Flutter本身只是一个UI系统，它本身是无法提供一些系统能力，比如使用蓝牙、相机、GPS等，因此要在Flutter APP中调用这些能力就必须和原生平台进行通信。为此，Flutter中提供了一个平台通道（platform channel），用于Flutter和原生平台的通信。平台通道正是Flutter和原生之间通信的桥梁，它也是Flutter插件的底层基础设施。

Flutter使用了一个灵活的系统，允许您调用特定平台的API，无论在Android上的Java或Kotlin代码中，还是iOS上的ObjectiveC或Swift代码中均可用。

Flutter与原生之间的通信依赖灵活的消息传递方式：

* 应用的Flutter部分通过平台通道（platform channel）将消息发送到其应用程序的所在的宿主（iOS或Android）应用（原生应用）。
* 宿主监听平台通道，并接收该消息。然后它会调用该平台的API，并将响应发送回客户端，即应用程序的Flutter部分。

使用平台通道在Flutter(client)和原生(host)之间传递消息，如下图所示：

![7.jpg](https://i.loli.net/2019/11/17/dxeyFgozOifRMHP.png)

当在Flutter中调用原生方法时，调用信息通过平台通道传递到原生，原生收到调用信息后方可执行指定的操作，如需返回数据，则原生会将数据再通过平台通道传递给Flutter。值得注意的是消息传递是异步的，这确保了用户界面在消息传递时不会被挂起。

在客户端，[MethodChannel API](https://docs.flutter.io/flutter/services/MethodChannel-class.html) 可以发送与方法调用相对应的消息。 在宿主平台上，MethodChannel 在[Android API](https://docs.flutter.io/javadoc/io/flutter/plugin/common/MethodChannel.html) 和 [FlutterMethodChannel iOS API](https://docs.flutter.io/objcdoc/Classes/FlutterMethodChannel.html)可以接收方法调用并返回结果。这些类可以帮助我们用很少的代码就能开发平台插件。

>注意: 如果需要，方法调用(消息传递)可以是反向的，即宿主作为客户端调用Dart中实现的API。 quick_actions插件就是一个具体的例子。

### 如何获取平台信息

Flutter 中提供了一个全局变量defaultTargetPlatform来获取当前应用的平台信息，defaultTargetPlatform定义在"platform.dart"中，它的类型是TargetPlatform，这是一个枚举类，定义如下：
``` dart
enum TargetPlatform {
  android,
  fuchsia,
  iOS,
}
```
可以看到目前Flutter只支持这三个平台。我们可以通过如下代码判断平台：
``` dart
if(defaultTargetPlatform==TargetPlatform.android){
  // 是安卓系统，do something
  ...
}
...
```
由于不同平台有它们各自的交互规范，Flutter Material库中的一些组件都针对相应的平台做了一些适配，比如路由组件MaterialPageRoute，它在android和ios中会应用各自平台规范的切换动画。那如果我们想让我们的APP在所有平台都表现一致，比如希望在所有平台路由切换动画都按照ios平台一致的左右滑动切换风格该怎么做？Flutter中提供了一种覆盖默认平台的机制，我们可以通过显式指定debugDefaultTargetPlatformOverride全局变量的值来指定应用平台。比如：
``` dart
debugDefaultTargetPlatformOverride=TargetPlatform.iOS;
print(defaultTargetPlatform); // 会输出TargetPlatform.iOS
```
上面代码即在Android中运行后，Flutter APP就会认为是当前系统是iOS，Material组件库中所有组件交互方式都会和iOS平台对齐，defaultTargetPlatform的值也会变为TargetPlatform.iOS。

## Flutter Web, Flutter Desktop

这些还在开发当中，鉴于对 Dart 喜欢，以及对 Flutter 性能的乐观，这些倒是很值得期待。

还记得平台只是给 Flutter 提供一个画布么，Flutter Desktop 未来更是可以大有可为的，相关可以看[这里](https://github.com/flutter/flutter/wiki/Desktop-shells)

最后每种方案，每种技术都有优缺点，甚至技术的架构决定了，有些缺陷可能永远都没法改进，现在的Flutter圈子 和社区比RN来说 还是比较小的，所以。。。


