## Dart 面向对象

### 类的简单介绍

创建一个类和创建类的实例

``` dart
void test1(){
  Dog d = new Dog();
}
class Dog {

}


var cat = new Cat("逗比", 12);
class Cat {
  String name;
  int age;

  Cat(String name, int age) {
    this.name = name;
    this.age = age;
  }
}

```

### 构造函数

普通构造函数

``` dart
var cat = new Cat("逗比", 12);

class Cat {
  String name;
  int age;

  Cat(String name, int age) {
    this.name = name;
    this.age = age;
  }
}

```

命名构造函数

给构造函数提供了名称，这样做使得不同的构造函数变的更加清晰。

``` dart
Map map = new Map();
map['name']= "哈巴狗";
map['age'] = 5;
Dog d = new Dog.newBorn(map);


class Dog {
  String name;
  int age;

  Dog(this.name, this.age);

  Dog.newBorn(Map json) {
    name = json['name'];
    age = json['age'];
  }
}

```

### 继承
可以使用extends关键字继承其他的类。

Pug 类继承Dog类，通过super关键字调用Dog类的构造函数。

``` dart
Pug p = new Pug('哈哈哈', 5);
print(p.name);


class Dog {
  String name;
  int age;

  Dog(this.name, this.age);

  Dog.newBorn() {
    name = 'Doggy';
    age = 0;
  }
}

class Pug extends Dog {
  Pug(String name, int age): super(name, age);
}

```

也可以通过this关键字，在冒号之后调用同一个类中的其他构造函数。

定义了两个命名构造函数，他们只需要dog的名字，然后调用Pug的默认构造函数

``` dart
Pug p = new Pug.small('哈哈哈');
print(p.name);

class Dog {
  String name;
  int age;

  Dog(this.name, this.age);

  Dog.newBorn() {
    name = '逗比';
    age = 0;
  }
}

class Pug extends Dog {
  Pug(String name, int age): super(name, age);

  Pug.small(String name): this(name, 1);

  Pug.large(String name): this(name, 3);
}

```

### 重载和重写

方法重写

代码如下，最后打印值是：你真是个逗比

``` dart
Pug p = new Pug();
print(p.bark());

class Dog {
  bark() {
    print('Bow Wow');
  }
}

class Pug extends Dog {
  @override
  bark() {
    print('你真是个逗比!');
  }
}

```

### 抽象类

* 可以通过abstract关键字声明抽象类

* 只需要在类声明前添加abstract关键字，方法不需要。方法只需要签名，不需要实现。

``` dart
abstract class AbstractDog {
  void eat();
  void _hiddenMethod();
}

class SmallDog extends AbstractDog{
  @override
  void _hiddenMethod() {
    
  }

  @override
  void eat() {
    
  }
}

```

### 访问权限

* 默认类中的所有属性和方法是public的。在dart中，可以在属性和方法名前添加“_”使私有化。现在让我们使name属性私有化。
* 可以发现，调用私有化变量或者方法的时候会出现红色警告

``` dart
  void test() {
    Dog d = new Dog('哈巴狗', 5);
    //这个报错
    print(d.name);
    print(d.age);
  }

```

Dog代码如下所示

``` dart
class Dog {
  String _name;
  int age;

  Dog(this._name, this.age);

  String get respectedName {
    return 'Mr.$_name';
  }

  set respectedName(String newName) {
    _name = newName;
  }

  Dog.newBorn() {
    _name = '哈巴狗';
    age = 0;
  }

  bark() {
    print('Bow Wow');
  }

  _hiddenMethod() {
    print('I can only be called internally!');
  }
}

```

### 静态方法

如果想让方法或者属性静态化，只需要在声明前添加static关键字。

``` dart
void test() {
  Dog.bark();
}


class Dog {
  static bark() {
    print('Bow Wow');
  }
}

```

### 泛型

dart全面支持泛型。假设你想在你定义的类中，想持有任意类型的数据。

``` dart
DataHolder<String> dataHolder = new DataHolder('Some data');
print(dataHolder.getData());
dataHolder.setData('New Data');
print(dataHolder.getData());
//下面这个会报错，因为dataHolder对象在创建的时候就已经限制为String类型
dataHolder.setData(123);
print(dataHolder.getData());

class DataHolder<T> {
  T data;

  DataHolder(this.data);

  getData() {
    return data;
  }

  setData(data) {
    this.data = data;
  }
}

```

### async/await介绍

思考一下，看了上面的案例，对于future的预期行为，如果我们希望在执行其他语句之前，先执行future，该怎么操作呢？

这就需要用到需要用到async/await。在test函数的花括号开始添加async关键字。我们添加await关键字在调用getTest方法之前，他所做的就是在future返回值之后，继续往下执行。我们将整个代码包裹在try-catch中，我们想捕获所有的异常，和之前使用catchError回调是一样。使用awiat关键字，必须给函数添加async关键字，否则没有效果。

注意：要使用 await，其方法必须带有 async 关键字。可以使用 try, catch, 和 finally 来处理使用 await 的异常！

``` dart 
  Future test() async {
    try {
      String value = await getTest();
      print("测试----------"+value);
    } catch(e) {
      print('测试----------Error');
    }
    print('测试----------逗比是这个先执行吗');
  }

  Future<String> getTest() {
    return new Future<String>.delayed(new Duration(milliseconds: 2000),() {
      return "This is a doubi";
    });
  }
  
  2019-06-21 17:32:37.701 16501-16583/com.hwmc.auth I/flutter: 测试----------This is a doubi
  2019-06-21 17:32:37.702 16501-16583/com.hwmc.auth I/flutter: 测试----------逗比是这个先执行吗

```

### 看一个案例

一个 async 方法 是函数体被标记为 async 的方法。 虽然异步方法的执行可能需要一定时间，但是 异步方法立刻返回 - 在方法体还没执行之前就返回了

``` dart 
void getHttp async {
    // TODO ---
}
```
在一个方法上添加 async 关键字，则这个方法返回值为 Future。

例如，下面是一个返回字符串的同步方法：
``` dart
String loadAppVersion() => "1.0.2"

```

使用 async 关键字，则该方法返回一个 Future，并且 认为该函数是一个耗时的操作。

``` dart
Futre<String> loadAppVersion() async  => "1.0.2"
```
注意，方法的函数体并不需要使用 Future API。 Dart 会自动在需要的时候创建 Future 对象。

好的代码是这样的
``` dart
void main() {
 //调用异步方法
 doAsync();
}

// 在函数上声明了 async 表明这是一个异步方法
Future<bool> doAsync() async {
  try {
    // 这里是一个模拟请求一个网络耗时操作
    var result = await getHttp();
    //请求出来的结果
    return printResult(result);
  } catch (e) {
    print(e);
    return false;
  }
}
//将请求出来的结果打印出来
Future<bool> printResult(summary) {
  print(summary);
}

//开始模拟网络请求 等待 5 秒返回一个字符串
getHttp() {
 return new Future.delayed(Duration(seconds: 5), () => "Request Succeeded");
}

```

不好的写法

``` dart
void main() {
 doAsync();
}

Future<String> doAsync() async {
    return  getHttp().then((r){
      return printResult(r);
    }).catchError((e){
      print(e);
    });
}

Future<String> printResult(summary) {
  print(summary);
}

Future<String> getHttp() {
 return new Future.delayed(Duration(seconds: 5), () => "Request Succeeded");
}

```

## Dart异常捕获

### 异常处理形式

dart 使用经典的try-catch处理异常，使用关键字throw抛出一个异常。

``` dart
  void test1(){
    divide(10, 0);
  }

  divide(int a, int b) {
    if (b == 0) {
      throw new IntegerDivisionByZeroException();
    }
    return a / b;
  }

```

当b变量的值为0的时候，抛出一个内置的异常IntegerDivisionByZeroException。

如何定义异常日志呢？

可以在异常中携带一个字符串信息。

``` dart
  void test1(){
    divide(10, 0);
  }

  divide(int a, int b) {
    if (b == 0) {
      throw new Exception('逗比，不能为0的');
    }
    return a / b;
  }

```

### 捕获异常

某种类型的异常可以通过on关键字捕获，如下：

``` dart
  void test1(){
    try {
      divide(10, 0);
    } on IntegerDivisionByZeroException {
      print('逗比，异常被捕获了');
    }
  }

  divide(int a, int b) {
    if (b == 0) {
      throw new IntegerDivisionByZeroException();
    }
    return a / b;
  }

```

注意问题，捕获的异常层级要大于抛出的异常，否则捕获会失败

还是会抛出异常'逗比，不能为0的'，因为Exception比
IntegerDivisionByZeroException层级要高

``` dart
  void test1(){
    try {
      divide(10, 0);
    } on IntegerDivisionByZeroException {
      print('逗比，异常被捕获了');
    }
  }

  divide(int a, int b) {
    if (b == 0) {
      throw new Exception('逗比，不能为0的');
    }
    return a / b;
  }

```

如果你不知道抛出异常的类型，或者不确定，可以使用catch块处理任意类型的异常。

``` dart
  void test1(){
    try {
      divide(10, 0);
    } on IntegerDivisionByZeroException {
      print('逗比，异常被捕获了');
    } catch (e) {
      print(e);
    }
  }

  divide(int a, int b) {
    if (b == 0) {
      throw new Exception('yc other exception.');
    }
    return a / b;
  }

```

### Finally讲解

dart也提供了finally块，即是否发生异常这个块都会执行。

``` dart
  void test1(){
    try {
      divide(10, 0);
    } on IntegerDivisionByZeroException {
      print('逗比，异常被捕获了');
    } catch (e) {
      print(e);
    }finally {
      print('I will always be executed!');
    }
  }

  divide(int a, int b) {
    if (b == 0) {
      throw new Exception('yc other exception.');
    }
    return a / b;
  }

```

## Dart枚举

### 枚举使用

dart 支持枚举，用法和java一样。

``` dart
Dog d = new Dog('哈巴狗', 12, CurrentState.sleeping);
print(d.state == CurrentState.sleeping); //Prints 'true'


enum CurrentState {
  sleeping,
  barking,
  eating,
  walking
}

class Dog {
  String name;
  int age;
  CurrentState state;

  Dog(this.name, this.age, this.state);

  static bark() {
    print('Bow Wow');
  }
}

```

### 元数据

使用元数据给代码添加额外信息，元数据注解是以@字符开头，后面是一个编译时常量或者调用一个常量构造函数。

有三个注解所有的Dart代码都可使用：@deprecated、@override，@proxy,

元数据可以在library、typedef、type parameter、constructor、factory、function、field、parameter、或者variable声明之前使用，也可以在import或者export指令之前使用，使用反射可以再运行时获取元数据信息。

###  自定义注解

定义自己的元数据注解。下面的示例定义一个带有两个参数的@toDo注解：

``` dart
void test1() {
  doSomething();
}


@toDo('seth', 'make this do something')
void doSomething() {
  print('do something');
}

class toDo {
  final String who;
  final String what;
  const toDo(this.who, this.what);
}

```

## Dart 字符串

### String简单介绍

Dart字符串是UTF-16编码的字符序列，可以使用单引号或者双引号来创建字符串：

``` dart
String str1 = '单引号字符串';
String str2 = "双引号字符串";

print(str1);        //输出：单引号字符串
print(str2);        //输出：双引号字符串

```

### 单双引号互相嵌套
String中单、双引号互相嵌套情况如下所示
``` dart
String str1 = '单引号中的"双引号"字符串';
String str2 = "双引号中的'单引号'字符串";

print("yc-str1--" + str1);
print("yc-str2--" + str2);

//单引号里面有单引号，必须在前面加反斜杠
String str3 = '单引号中的\'单引号\'';
String str4 = "双引号里面有双引号,\"双引号\"";
print("yc-str3--" + str3);
print("yc-str4--" + str4);

```

注意点:：

单引号嵌套单引号之间不允许出现空串（不是空格），双引号嵌套双引号之间不允许出现空串：
 
``` dart
//String str5 = '单引号''''单引号';  //报错了，逗比
String str6 = '单引号'' ''单引号';
String str7 = '单引号''*''单引号';
String str8 = "双引号"" ""双引号";
String str9 = "双引号""*""双引号";
//String str10 = "双引号""""双引号";   //报错了，逗比
print("yc-str6--" + str6);
print("yc-str7--" + str7);
print("yc-str8--" + str8);
print("yc-str9--" + str9);

```

