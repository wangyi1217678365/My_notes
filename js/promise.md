## Promise（是异步编程的一种解决方案）

### Promise 出现的原因
是为了解决回调地狱  
回调地狱带来的负面作用：
- 代码臃肿。
- 可读性差。
- 耦合度过高，可维护性差。
- 代码复用性差。
- 容易滋生 bug。
- 只能在回调里处理异常。  
  
Promise 是异步编程的一种解决方案：
从语法上讲，promise是一个对象，从它可以获取异步操作的消息；从本意上讲，它是承诺，承诺它过一段时间会给你一个结果。
promise有三种状态：pending(等待态)，fulfiled(成功态)，rejected(失败态)；状态一旦改变，就不会再变。创造promise实例后，它会立即执行。

### 手写Promise
1. **Promise 声明**  
    首先呢，promise肯定是一个类，我们就用class来声明。
    - 由于new Promise((resolve, reject)=>{})，所以传入一个参数（函数），秘籍里叫他executor，传入就执行。
    - executor里面有两个参数，一个叫resolve（成功），一个叫reject（失败）。
    - 由于resolve和reject可执行，所以都是函数，我们用let声明。
    ```
    class Promise {
        constructor (executor) {
            // 成功
            let resolve = function () {}
            // 失败
            let reject = function () {}
            // 立即执行
            executor(resolve, reject);
        }
    }
    ```
2. **解决基本状态**
   - Promise存在三种状态（state）padding、fulfilled、rejected
   - padding（等待态）为初始状态，并可转化为fulfilled（成功）、rejected（失败）
   - 成功时，不可转变为其它状态，且b必须有一个不可改变的值（value）
   - 失败时，不可转变为其它状态，且b必须有一个不可改变的值（reason）
   - new Promise((resolve, reject)=>{resolve(value)}) resolve为成功，接收参数value，状态改变为fulfilled，不可再次改变。
   - new Promise((resolve, reject)=>{reject(reason)}) reject为失败，接收参数reason，状态改变为rejected，不可再次改变。
   - 若是executor函数报错 直接执行reject();
  ```
    class Promise{
        constructor(executor){
            // 初始化state为等待态
            this.state = 'pending';
            // 成功的值
            this.value = undefined;
            // 失败的原因
            this.reason = undefined;
            let resolve = value => {
            // state改变,resolve调用就会失败
            if (this.state === 'pending') {
                // resolve调用后，state转化为成功态
                this.state = 'fulfilled';
                // 储存成功的值
                this.value = value;
            }
            };
            let reject = reason => {
            // state改变,reject调用就会失败
            if (this.state === 'pending') {
                // reject调用后，state转化为失败态
                this.state = 'rejected';
                // 储存失败的原因
                this.reason = reason;
            }
            };
            // 如果executor执行报错，直接执行reject
            try{
                executor(resolve, reject);
            } catch (err) {
                reject(err);
            }
        }
    }
  ```
### then方法
规定:Promise有一个叫做then的方法，里面有两个参数：onFulfilled,onRejected,成功有成功的值，失败有失败的原因
- 当状态state为fulfilled，则执行onFulfilled，传入this.value。当状态state为rejected，则执行onRejected，传入this.reason
- onFulfilled,onRejected如果他们是函数，则必须分别在fulfilled，rejected后被调用，value或reason依次作为他们的第一个参数
```
class Promise{
  constructor(executor){...}
  // then 方法 有两个参数onFulfilled onRejected
  then(onFulfilled,onRejected) {
    // 状态为fulfilled，执行onFulfilled，传入成功的值
    if (this.state === 'fulfilled') {
      onFulfilled(this.value);
    };
    // 状态为rejected，执行onRejected，传入失败的原因
    if (this.state === 'rejected') {
      onRejected(this.reason);
    };
  }
}
```
### 解决异步实现
现在基本可以实现简单的同步代码，但是当resolve在setTomeout内执行，then时state还是pending等待状态 我们就需要在then调用的时候，将成功和失败存到各自的数组，一旦reject或者resolve，就调用它们  
类似于发布订阅，先将then里面的两个函数储存起来，由于一个promise可以有多个then，所以存在同一个数组内。
```
    // 多个then的情况
    let p = new Promise();
    p.then();
    p.then();
```
成功或者失败时，forEach调用它们
```
class Promise{
  constructor(executor){
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    // 成功存放的数组
    this.onResolvedCallbacks = [];
    // 失败存放法数组
    this.onRejectedCallbacks = [];
    let resolve = value => {
      if (this.state === 'pending') {
        this.state = 'fulfilled';
        this.value = value;
        // 一旦resolve执行，调用成功数组的函数
        this.onResolvedCallbacks.forEach(fn=>fn());
      }
    };
    let reject = reason => {
      if (this.state === 'pending') {
        this.state = 'rejected';
        this.reason = reason;
        // 一旦reject执行，调用失败数组的函数
        this.onRejectedCallbacks.forEach(fn=>fn());
      }
    };
    try{
      executor(resolve, reject);
    } catch (err) {
      reject(err);
    }
  }
  then(onFulfilled,onRejected) {
    if (this.state === 'fulfilled') {
      onFulfilled(this.value);
    };
    if (this.state === 'rejected') {
      onRejected(this.reason);
    };
    // 当状态state为pending时
    if (this.state === 'pending') {
      // onFulfilled传入到成功数组
      this.onResolvedCallbacks.push(()=>{
        onFulfilled(this.value);
      })
      // onRejected传入到失败数组
      this.onRejectedCallbacks.push(()=>{
        onRejected(this.reason);
      })
    }
  }
}
```
### 解决链式调用
我门常常用到new Promise().then().then(),这就是链式调用，用来解决回调地狱  
1. 为了达成链式，我们默认在第一个then里返回一个promise。秘籍规定了一种方法，就是在then里面返回一个新的promise,称为promise2：promise2 = new Promise((resolve, reject)=>{})
   - 将这个promise2返回的值传递到下一个then中
   - 如果返回一个普通的值，则将普通的值传递给下一个then中
2. 当我们在第一个then中return了一个参数（参数未知，需判断）。这个return出来的新的promise就是onFulfilled()或onRejected()的值   
规定onFulfilled()或onRejected()的值，即第一个then返回的值，叫做x，判断x的函数叫做resolvePromise  
- 首先，要看x是不是promise。
- 如果是promise，则取它的结果，作为新的promise2成功的结果
- 如果是普通值，直接作为promise2成功的结果
- 所以要比较x和promise2
- resolvePromise的参数有promise2（默认返回的promise）、x（我们自己return的对象）、resolve、reject
- resolve和reject是promise2的
