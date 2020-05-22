## 简答题

### 1. 请说出下列最终的执行结果，并解释为什么？

```javascript
var a = []
for (var i = 0; i < 10; i++){
    a[i] = function(){
        console.log(i)
    }
}
a[6]()
```

在`for`中使用`var`定义了一个全局变量`i`，当循环结束`i`变成10的时候调用`a[6]`方法打印`i`的结果自然是10。

### 2. 请说出下列最终的执行结果，并解释为什么？

```javascript
var temp = 123
if(true){
    console.log(temp)
    let temp
}
```

在`if`的块中使用`let`定义了`temp`，且`let`不能声明提升，所以在定义之前打印会报错：`Cannot access 'temp' before initialization`。

### 3. 结合ES6新语法，用最简单的方式找出数组中的最小值？

```javascript
var arr = [12, 34, 32, 89, 4]
const minNum = Math.min(...arr)
```

### 4. 请说明`var`，`let`，`const`三种声明变量的方式之间的具体差别？

- var：函数作用域；变量声明提升；可以重复定义；声明的变量会作为window的属性。
- let：块级作用域；不会声明提升；不可以重复定义；不会作为window的属性。
- const：在let的基础上添加了只读（浅层只读，深层可读写）的属性。再声明的时候必须同时进行初始化。

### 5. 请说出下列最终的执行结果，并解释为什么？

```javascript
var a = 10
var obj = {
    a: 20,
    fn(){
        setTimeout(() => {
            console.log(this.a)
        })
    }
}
obj.fn()
```

传统定义函数的`this`会指向调用它的对象，所以`fn()`执行的时候的``this``会指向obj，箭头函数不会影响`this`指向，其`this`为父级`this`，所以这里箭头函数中的`this`也是指向`obj`的，所以打印`20`。

### 6. 简述symbol类型的用途？

1. 使用Symbol作为属性名避免属性名冲突。
2. 模拟实现对象的私有成员。

### 7. 什么是深拷贝什么是浅拷贝？

深拷贝复制的是地址中的数据，浅拷贝复制的是地址值。

### 8. 谈谈你是如何理解JS异步编程的，Eventloop是做什么的，什么是宏任务，什么是微任务？

**异步编程：** 异步编程就是将某些比较耗时的任务托管到另一个线程处理，使其不会阻塞主线程任务的执行。防止耗时任务导致的程序假死现象。由于j一般是通过回调函数的方式定义后续的程序执行方式

**宏任务：** 每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）。

**微任务：** 在当前宏任务执行结束后立即执行的任务。在当前宏任务后，下一个之前，且是在渲染之前，就会将在它执行期间产生的所有微任务都执行完毕。

### 9. 将下面异步代码使用Promise改进？

```javascript
setTimeout(function(){
    var a = 'hello'
    setTimeout(function(){
        var b = 'lagou'
        setTimeout(function(){
            var c = 'I ♥ U'
            console.log(a + b + c)
        }, 10)
    }, 10)
}, 10)
```

```javascript
const promise1 = new Promise(resolve => {
    setTimeout(function(){
        const a = 'hello'
        resolve(a)
    }, 10)
})

const promise2 = new Promise(resolve => {
    setTimeout(function(){
        const b = 'hello'
        resolve(b)
    }, 10)
})

Promise.all([promise1, promise2]).then(res => {
    setTimeout(function(){
        var c = 'I ♥ U'
        console.log(res[0] + res[1] + c)
    }, 10)
})
```