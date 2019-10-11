
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
``` 
var t;
t="yc";
// 下面代码在dart中会报错，因为变量t的类型已经确定为String，
// 类型一旦确定后则不能再更改其类型。
t=1000;
```
最大的不同是Dart中var变量一旦赋值，类型便会确定，则不能再改变其类型。因为Dart本身是一个强类型语言，任何变量都是有确定类型的，在Dart中，当用var声明一个变量后，Dart在编译时会根据第一次赋值数据的类型来推断其类型，编译结束后其类型就已经被确定。

### 变量
``` 
var num = 0;
var title = "标题";
```
>Dart 不需要给变量设置 setter getter   方法， 这和 kotlin 等类似。Dart 中所有的基础类型、类等都继承 Object ，默认值是 NULL， 自带 getter 和 setter ，而如果是 final 或者 const 的话，那么它只有一个 getter 方法。

### 常量
``` 
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
``` 
var num = 0;
var title = "标题";
var mIsLogin = false;

```
>List 就是数组
``` 
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
```
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
```
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
```
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

（未完待续）
