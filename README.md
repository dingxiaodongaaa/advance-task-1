# 丁晓东 ｜ Part 1 | 模块一

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

**异步编程：** JS是单线程的，所以任务只能一个执行完在执行下一个，如果有任务非常的耗时间（比如或延时器），其后续的任务就只能等待，一直到耗时任务结束之后再去执行，使程序出现假死的现象。js异步编程可以将某些比较耗时的任务托管到另一个线程处理，使其不会阻塞主线程后续任务的执行。防止耗时任务导致的程序假死现象。

**Eventloop：** Eventloop可以说是js异步任务执行的调度中心，它会捕获到js线程的异步任务，并将异步任务分配给相应的线程去处理，当异步任务处理完成，又会将结果返回给js线程，js线程就调用事先设定好的回调函数，完成整个任务。

**宏任务：** 每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）。

```
script(整体代码)
setTimeout
setInterval
I/O
UI交互事件
postMessage
MessageChannel
setImmediate(Node.js 环境)
```

**微任务：** 在当前任务执行结束后立即执行的任务。在当前任务后，下一个任务之前，且是在渲染之前，就会将在它执行期间产生的所有微任务都执行完毕。

```
Promise.then
Object.observe
MutaionObserver
process.nextTick(Node.js 环境)
```

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

分析得，第一层和第二层异步程序之间没有依赖关系，第三层异步程序依赖于第一层和第二层的结果，所以可以用`Promise.all()`去并发执行前两层的异步程序，然后串行执行第三层程序。

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
        const c = 'I ♥ U'
        console.log(res[0] + res[1] + c)
    }, 10)
})
```

### 10. 请简述TypeScript和JavaScript之间的关系？

TypeScript是JavaScript的超集，解决js自有类型系统的不足，提高代码的可靠性。TypeScript需要编译成JavaScript再去执行。

### 11. 请谈谈你所认识的TypeScript优缺点。

**优势：** 

1. TypeScript有完善的类型系统，可以在程序运行之前就发现解决类型错误，避免开发过程中可能出现的类型异常，提高编码效率和代码可靠性。
2. TypeScript对于ECMAScript的新特性有良好的支持,即便不使用TypeScript的类型系统，也可以使用TypeScript来对ECMAScript的新特性提供支持。
3. 由于TypeScript会编译成JavaScript工作，所以任何的JavaScript运行环境都可以使用TypeScript开发。
4. 开发工具对于TypeScript的支持性好。

**缺点：**

1. 语言本身多了很多的概念（接口、泛型、枚举等）提高了TS的学习成本。
2. 对于小项目或者项目初期，TypeScript会增加项目的开发成本。
