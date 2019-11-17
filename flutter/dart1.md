
# Dart语言

## 1.Dart重要概念
Dart语言是Google在2011年10月在丹麦举行的Goto会议上宣布，是一种结构化的web编程语言，因为Dart语言拥有一些特性，例如： AOT编辑，JIT编辑，可以轻松的创建60fps运行的流畅动画和转场。不需要单独的声明式布局语言，易学习等特点。 这使得Flutter 使用了Dart这个语言 成为其开发语言。Dart拥有几个概念：
* 所有东西都是对象，所有的对象都继承自内置的Object类。
* 程序中指定数据类型使得程序合理的分配内存空间，并且帮助编译器进行语法检查。但是，指定类型不是必须的。Dart 是弱类型语言。
* Dart代码在运行前解析。
* Dart程序有统一的入口 main() 这一点和C C++ 很像。
* Dart语言没有public protected 和 private 的概念。 私有特性  通过 变量 或者函数 加上下划线表示。
* Dart语言支持async/await 异步处理

## 变量 基本数据类型

### var声明变量
> 类似于kotlin,JS中的var，它可以接收任何类型的变量，但最大的不同是Dart中var变量一旦赋值，类型便会确定，则不能再改变其类型，如：
```  dart
var t;
t="yc";
// 下面代码在dart中会报错，因为变量t的类型已经确定为String，
// 类型一旦确定后则不能再更改其类型。
t=1000;
```
最大的不同是Dart中var变量一旦赋值，类型便会确定，则不能再改变其类型。因为Dart本身是一个强类型语言，任何变量都是有确定类型的，在Dart中，当用var声明一个变量后，Dart在编译时会根据第一次赋值数据的类型来推断其类型，编译结束后其类型就已经被确定。

### 变量
```  dart
var num = 0;
var title = "标题";
```
>Dart 不需要给变量设置 setter getter   方法， 这和 kotlin 等类似。Dart 中所有的基础类型、类等都继承 Object ，默认值是 NULL， 自带 getter 和 setter ，而如果是 final 或者 const 的话，那么它只有一个 getter 方法。

### 常量
```  dart
//final 表示常量 只能设定一次
final name = "111";
// name = '222' 会引发一个错误

//static const 组合代表了静态常量
static const String complete = "COMPLETE";
```
>final和const区别
两者区别在于：const 变量是一个编译时常量，final变量在第一次使用时被初始化。被final或者const修饰的变量，并且变量类型可以省略。

### 基本数据类型

Dart语言的常用的基本数据类型包括： Number String  Boolean List Map
Number  String  Boolean  在JS中 很熟悉了。 用var 进行变量赋值的时候会推断成对应的类型
```  dart
var num = 0;
var title = "标题";
var mIsLogin = false;

```
>List 就是数组
```  dart
  List arr1 = [1,2,3,4];
  var arr2 = [1,2,3,4];
 
  print(list); //Output: [1, 2, 3, 4]
  //Length 长度
  print(list.length);
 
  //Selecting single value 获取单个值
  print(list[1]);    //Outout: 2
 
  //Adding a value 添加值到list
  list.add(10);
 
  //Removing a single isntance of value 删除单个值
  list.remove(3);
 
  //Remove at a particular position 删除指定位置的值 第一个元素索引是0，最后一个元素是length-1

  list.removeAt(0);

```
>Map
```  dart
    var map = {
      'key1': 'value1',
      'key2': 'value2',
      'key3': 'value3'
    };
    //Fetching the values 获取值
    print(map['key1']);    //Output: value1
    print(map['test']);    //Output: null

    //Add a new value 添加值
    map['key4'] = 'value4';

    //Length   获取长度
    print(map.length);

    //Check if a key is present 检查是否存在
    var containsKey = map.containsKey('value1');
    print(containsKey);

    var entries = map.entries;
    var values = map.values;
```
也可以使用map构造函数定义map。
```  dart
var squares = new Map();
squares["a"] = 1;
squares["b"] = 2;
squares["c"] = 3.0;
squares["d"] = [1,2];
squares["e"] = "yc逗比";

print(squares['a']);    
print(squares['e']);
```

## 函数

dart中的函数和JavaScript中有点类似。你需要定义就是函数的名字、返回值(有返回值或者void)、参数。
```  dart
void test(){
    var name = fullName('杨充', '逗比');
    print(name);
  }

  String fullName(String firstName, String lastName) {
    return "$firstName $lastName";
  }
```
### 参数默认值

你可以给函数的命名参数一个默认值。下面的例子给lastName一个默认值。

``` dart
  void test(){
    var name = fullName('杨充', '逗比');
    print(name);
  }

  fullName(String firstName, String lastName) {
    return "$firstName $lastName";
  }

```
###  void无返回值
大多数都是void无返回值的函数，这个跟java中类似。没什么好讲的……

### 匿名函数
在dart中函数比较灵活，例如，你可以将函数当参数传递给另一个函数。
``` dart
void test(){
    out(printOutLoud);
  }

  out(void inner(String message)) {
    inner('Message from inner function');
  }

  printOutLoud(String message) {
    print(message.toUpperCase());
  }
```
* 这里定义一个函数名字为out,需要一个函数参数。然后我定义一个名为printOutLoud的函数，他所做的就是将字符串以大写的形式打印。
* dart 也有匿名函数，所以上面的例子中不用预定一个函数，而是传递一个匿名函数。

### 运算符
和java  js 语言类似 不再赘述

## 流程控制 
大概有这么多 很多都和JS 相同
* if和else
``` dart
var number = 57;
if (number > 100) {
  print('Large Number');
} else if (number < 100) {
  print('Small Number');
} else {
  print('Number is 100');
}
/// 三元运算符
int age = 60;
String status = age < 50 ? "年轻人" : "老年人";
```
* for循环
``` dart
  void test() {
    for (int i = 0; i < 10; i++) {
      print('$i');
    }
  }

```
* while循环
``` dart
  void test() {
    int i = 0;
    while(i < 10) {
      print('$i');
      i++;
    }
  }
/// do while
void test() {
    int i = 0;
    do {
      print('$i');
      i++;
    } while (i < 10);
  }
```
* break和continue
* switch和case
``` dart
void test() {
    int age = 50;
    switch(age) {
      case 10:
        print('Too Young.');
        break;
      case 20:
      case 30:
        print('Still Young!');
        break;
      case 40:
        print('Getting old.');
        break;
      case 50:
        print('You are old!');
        break;
    }
  }
```
* assert断言

## Dart语言中的异步

### Future介绍
* async 库中有一个叫Future的东西。Future是基于观察者模式的。如果你熟悉Rx或者JavaScript的Promises，你就很容易明白了。

首先先看一下下面的案例，看看它们之间有什么区别？
``` dart
  void testA() async{
    new Future<String>(() {
      return "This is a doubi";
    });
  }

  Future testB() async{
    return new Future<String>(() {
      return "This is a doubi";
    });
  }

  Future<String> testC() {
    return new Future<String>(() {
      return "This is a doubi";
    });
  }

```
### 普通异步案例

* Future是支持泛型的，例如Future,通过T指定将来返回值的类型。

* 定义了一个叫getTest的函数，返回值为Future.你可以通过new关键字创建一个Future。Future的构造函数，需要一个函数作为参数，这个函数返回T类型的数据。在匿名函数中的返回值就是Future的返回值。
* 当调用了getTest方法，他返回Future.我们通过调用then方法订阅Future，在then中注册回调函数，当Future返回值时调用注册函数。同时注册了catchError方法处理在Future执行之间发生的异常。这个例子中不会发生异常。
``` dart
  void test() {
    getTest().then((value) {
      print("测试----------"+value);
    }).catchError((error) {
      print('测试----------Error');
    });
  }

  Future<String> getTest() {
    return new Future<String>(() {
      return "This is a doubi";
    });
  }
  
  //打印结果
  2019-06-21 17:11:12.941 16501-16583/com.hwmc.auth I/flutter: 测试----------This is a doubi

```
* 下面这个案例会发生异常
``` dart
  void test() {
    getTest().then((value) {
      print("测试----------"+value);
    }).catchError((error) {
      print('测试----------Error');
    });
  }

  Future<String> getTest() {
    return new Future<String>(() {
      return "This is a doubi";
    });
  }
  
  //打印结果
  2019-06-21 17:18:46.896 16501-16583/com.hwmc.auth I/flutter: 测试----------Error

```
### 耗时异步案例
* 在生产环境中都是一些耗时的操作，例如，网络调用，我们可以使用Future.delayed()模仿。
* 现在如果你运行，你将需要2秒，才能返回结果。
``` dart
  void test() {
    getTest().then((value) {
      print("测试----------"+value);
    }).catchError((error) {
      print('测试----------Error');
    });
  }

  Future<String> getTest() {
    return new Future<String>.delayed(new Duration(milliseconds: 2000),() {
      return "This is a doubi";
    });
  }

```
* 接下来再看一个案例。在调用函数之后，我们添加了print语句。在这种场景中，print语句会先执行，之后future的返回值才会打印。这是future的预期行为.但是如果我们希望在执行其他语句之前，先执行future。

``` dart 
  void test() {
    getTest().then((value) {
      print("测试----------"+value);
    }).catchError((error) {
      print('测试----------Error');
    });
    print('测试----------逗比是这个先执行吗');
  }

  Future<String> getTest() {
    return new Future<String>.delayed(new Duration(milliseconds: 2000),() {
      return "This is a doubi";
    });
  }
  
  2019-06-21 17:26:16.619 16501-16583/com.hwmc.auth I/flutter: 测试----------逗比是这个先执行吗
  2019-06-21 17:26:17.176 16501-16583/com.hwmc.auth I/flutter: 测试----------This is a doubi

```



