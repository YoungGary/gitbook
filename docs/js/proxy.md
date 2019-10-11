# Vue.js 3 中使用 Proxy 实现响应式

## 前言

10月5号凌晨，尤雨溪公布了 Vue 3 的[源代码](https://github.com/vuejs/vue-next)，目前的版本是 Pre-Alpha 。
我们知道Vue 的核心之一就是响应式系统，通过侦测数据的变化，来驱动更新视图。在Vue.js 2的版本中是通过 Object.defineProperty()函数来实现的响应式。大家早就得知在Vue新版的响应式是用 Proxy 实现的，现在我们来利用 Proxy 实现一个基本的响应式骨架。

## 基础

关于 Proxy 的基础知识，可以去MDN学习直达[链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

## 实现目标

实现一个响应式的核心函数命名为 reactive， 这个函数返回一个被代理之后的结果，可以通过操作这个返回结果来触发响应式。
比如实现一个有 name 属性的对象 obj，我们希望经过 reactive 函数处理之后返回一个新对象 proxyObj，我们像操作 obj 一样操作 proxyObj 对象。

``` javascript
let obj = {
    name: 'test name'
}

let proxyObj = reactive(obj);
```
例如：当修改 proxyObj.name 时会触发响应式。

其次：设置一个函数用来模拟响应式过程，这里省略掉更新dom的操作,用打印一句话来代替模拟一下。

``` javascript
// 提示视图需要更新
function trigger() {
    console.log('视图需要更新');
}
```
## 实现 reactive 函数

### 需要一个辅助函数
首先明确 reactive 函数接收一个参数，需要对这个参数进行代理，并返回代理后的结果。如果参数是对象才需要代理，否则直接返回。

这里需要建立一个辅助函数用来判断一个变量是否是对象：
``` javascript
function isObject(param) {
    return typeof param === 'object' && param !== null;
}
```

### 主体函数
使用 Proxy 时需要定义一个代理对象 handler 来对目标进行代理操作，这个对象主要有两个方法，即 get 和 set, 分别为 获取和设置属性值的时候触发。同时在内部的实现需要利用到 Reflect对象， 详情见下面的代码。

``` javascript
/* 返回一个被代理后的结果，通过操作这个结果可以来实现响应式, 例如视图更新 */
function reactive(target) {
    // 如果是个对象，则返回被代理后的结果，如果不是则直接返回
    if(!isObject(target)) {
        return target;
    }
    // 需要定义一个代理对象来对 target 进行代理操作
    // 这个对象主要有两个方法，即 get 和 set
    const handler = {
        get(target, key, receiver) {
            return Reflect.get(target, key, receiver); // 相当于 return target[key]
        },
        set(target, key, value, receiver) {
            trigger();
            return Reflect.set(target, key, value, receiver); // 相当于 target[key] = value
        }
    };

    // 利用 Proxy 来代理这个对象属性
    let observed = new Proxy(target, handler);

    return observed;
}
```
现在我们修改 proxyObj 的name属性，发现会触发了最基本的响应式：
``` javascript
let obj = {
    name: 'jjjs'
}

let proxyObj = reactive(obj);

// 修改 name 属性，发现可以监控到
proxyObj.name = 'new Name';
console.log(proxyObj.name);
//结果打印：
//视图需要更新
//new Name

```
而且可以对本来不存在的属性进行监控：
``` javascript
proxyObj.age = 6;
console.log(proxyObj.age);
//视图需要更新
//6
```
但是当我们想处理一个数组时，却发现会触发两次视图更新提示：
``` javascript
let array = [1,2,3];
let obArray = reactive(array);
obArray.push(4)  // --结果会出现2次   视图需要更新
```
这个时候对reactive代码进行修改，输出set动作中每次的 key, 观察是哪个键使得函数触发了两次
``` javascript
function reactive(target) {
    const handler = {
    // ...
        set(target, key, value, receiver) {
            trigger();
            console.log(key); // 输出变动的 key  是3 和length
            return Reflect.set(target, key, value, receiver); // 相当于 target[key] = value
        }
    };
    // ...
}

```
可以看出当监视数组时，数组下标的更新会触发一次，而数组length的更新也会进行触发，这就是二次触发的原因。
但我们是不需要在 length 更新时对视图进行更新的，所以需要对这里的逻辑进行修改：只对私有属性的修改动作触发视图更新。
``` javascript
function reactive(target) {
    const handler = {
        // ...
        set(target, key, value, receiver) {
            // 只对私有属性的修改动作触发视图更新
            if(!target.hasOwnProperty(key)) {
                trigger();
                console.log(key);
            }
            return Reflect.set(target, key, value, receiver); // 相当于 target[key] = value
        }
    };
    // ...
}
``` 
当需要对嵌套的对象进行获取时，例如：
``` javascript
// 对于嵌套的对象
var obj = {
    name: 'jjjs',
    array: [1,2,3]
}

var proxyObj = reactive(obj);
proxyObj.array.push(4);
```
此时会发现，并不会触发视图需要更新的提示，这时需要对对象进行递归处理：
``` javascript
function reactive(target) {
// ...
    const handler = {
        get(target, key, receiver) {
            const proxyTarget =  Reflect.get(target, key, receiver); // 相当于获取 target[key]
            if(isObject(target[key])) { // 对于对象进行递归
                return reactive(proxyTarget); // 递归
            }

            return proxyTarget;
        },
        // ...
    };
// ...
}

```
然而，当多次获取代理结果时，会出现多次触发代理的情况：
``` javascript
function reactive(target) {
// ...
    console.log('走代理');
    
    // 利用 Proxy 来代理这个对象属性
    let observed = new Proxy(target, handler);
    return observed;
}

// 多次获取代理结果
var proxyObj = reactive(obj);
var proxyObj = reactive(obj);
var proxyObj = reactive(obj);
var proxyObj = reactive(obj);

//结果输出 4次 走代理
```
这种情况是我们不希望有的，我们希望对同一个对象仅做一次代理。这个时候，我们需要对已经代理过的对象进行缓存，一次在进行代理之前查询缓存判断是否已经经过了代理，只有没有经过代理的对象才走一次代理。

最适合当做缓存容器的对象是 WeakMap， 这是由于它对于对象的弱引用特性，vue3中也是使用了weakmap来维护。

>Q:为什么用weekmap而不是map?
>A:weakmap是弱类型可以直接被回收

现在对代码进行修改，增加一个缓存对象：
``` javascript
const toProxy = new WeakMap(); // 用来保存代理后的对象

function reactive(target) {
    // ...
    if(toProxy.get(target)) { // 判断对象是否已经被代理了
        return toProxy.get(target);
    }
    // ...
    console.log('走代理');
    // 利用 Proxy 来代理这个对象属性
    let observed = new Proxy(target, handler);

    toProxy.set(target, observed); // 保存已经代理了的对象

    return observed;
}

``` 
发现对同一个对象只走了一次代理，这正是我们期望的结果。

### 完整代码

``` javascript
/* demo.js */

// 定义一个缓存对象
const toProxy = new WeakMap(); // 保存代理后的对象

/* 返回一个被代理后的结果，通过操作这个结果可以来实现响应式, 例如视图更新 */
function reactive(target) {
    // 如果是个对象，则返回被代理后的结果，如果不是则直接返回
    if(!isObject(target)) {
        return target;
    }
    
    if(toProxy.get(target)) {
        return toProxy.get(target);
    }

    const handler = {
        get(target, key, receiver) {
            const proxyTarget =  Reflect.get(target, key, receiver); // 相当于 return target[key]
            if(isObject(target[key])) {
                return reactive(proxyTarget);
            }

            return proxyTarget;
        },
        set(target, key, value, receiver) {
            // 只对私有属性的修改动作触发视图更新
            if(!target.hasOwnProperty(key)) {
                trigger();
            }
            return Reflect.set(target, key, value, receiver); // 相当于 target[key] = value
        }
    };

    // 利用 Proxy 来代理这个对象属性
    let observed = new Proxy(target, handler);

    toProxy.set(target, observed); // 保存已代理的对象

    return observed;
}

// 提示视图需要更新
function trigger() {
    console.log('视图需要更新');
}

function isObject(param) {
    return typeof param === 'object' && param !== null;
}
```
>建立一个页面来试验一下结果

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>reactive</title>
</head>
<body>
<script src="demo.js"></script>
<script>

// var obj = {
//     name: 'jjjs'
// }

// var proxyObj = reactive(obj);

// 修改 name 属性，发现可以监控到
// proxyObj.name = 'new Name';
// console.log(proxyObj.name);

// 对于本来不存在的属性也可以进行监控
// proxyObj.age = 6;
// console.log(proxyObj.age);


// 对于数组，会出现两次触发
// let array = [1,2,3];
// let obArray = reactive(array);
// obArray.push(4)

// 对于嵌套的对象
var obj = {
    name: 'jjjs',
    array: [1,2,3]
}

// 多次获取代理结果
var proxyObj = reactive(obj);
var proxyObj = reactive(obj);
var proxyObj = reactive(obj);
var proxyObj = reactive(obj);

// // 仅操作一次数组
// proxyObj.array.push(4);


</script>
</body>
</html>
```




