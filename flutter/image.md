# Flutter 图片组件
图片组件是显示图像的组件， Image 组件有很多构造函数， 这里我们只给大家讲两个

Image.asset， 本地图片
 * 第一步 在目录中创建文件夹，放入需要的图片
 * 第二步 打开 pubspec.yaml 声明一下添加的图片文件， 注意要配置对
```
 # To add assets to your application, add an assets section, like this:
  assets:
   - images/3.jpg
  #  - images/a_dot_ham.jpeg
```

 * 第三步 在代码中使用
 ``` dart
 class Home extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Image.asset("images/3.jpg",
        fit:BoxFit.cover
      ),
      width: 300.0,
      height: 400.0,
      decoration: BoxDecoration(
      color: Colors.yellow
      ),
    );
  }
}
 ```


Image.network 远程图片

``` dart
class Home extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Image.network(
        "http://pic.baike.soso.com/p/20130828/20130828161137-1346445960.jpg",
        alignment: Alignment.topLeft,
        color: Colors.red,
        colorBlendMode: BlendMode.colorDodge,
        // repeat: ImageRepeat.repeatX,
        fit: BoxFit.cover,
      ),
      width: 300.0,
      height: 400.0,
      decoration: BoxDecoration(
      color: Colors.yellow
      ),
    );
  }
}
```


