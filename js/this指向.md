# this指向
## this是什么
this不是指向自身！this 就是一个指针，指向调用函数的对象。
一定要注意，除非是箭头函数，否则this跟词法作用域是两回事，一定要牢记在心
## this的绑定规则
1. [默认绑定](#默认绑定)
2. [隐式绑定](#隐式绑定)
3. [显示绑定](#显示绑定)
4. [new绑定](#new绑定)
   
    ### 默认绑定 
    定义: 在不能应用其它绑定规则时使用的默认规则，通常是独立函数调用。
    ```
      function sayHi(){
          console.log('Hello,', this.name);
      }
      var name = 'YvetteLau';
      sayHi(); // 'Hello,YvetteLau'
    ```
    在调用sayHi()时，应用了默认绑定，this指向全局对象（非严格模式下），严格模式下，this指向undefined，undefined上没有this对象，会抛出错误。

    ### 隐式绑定
    定义: 函数的调用是在某个对象上触发的，即调用位置上存在上下文对象。典型的形式为 XXX.fun()
    `函数在调用位置，是否有上下文对象，如果有，那么this就会隐式绑定到这个对象上。`
    ```
      function sayHi(){
          console.log('Hello,', this.name);
      }
      var person = {
          name: 'YvetteLau',
          sayHi: sayHi
      }
      var name = 'Wiliam';
      person.sayHi(); // 'Hello,YvetteLau'
    ```
    sayHi函数声明在外部，严格来说并不属于person，但是在调用sayHi时,调用位置会使用person的上下文来引用函数，隐式绑定会把函数调用中的this(即此例sayHi函数中的this)绑定到这个上下文对象（即此例中的person）
    > 需要注意的是：只有对象属性链中只有最后一层会影响到调用位置。
    ```
      function sayHi(){
          console.log('Hello,', this.name);
      }
      var person2 = {
          name: 'Christina',
          sayHi: sayHi
      }
      var person1 = {
          name: 'YvetteLau',
          friend: person2
      }
      person1.friend.sayHi(); // 'Hello, Christina'
    ```
    因为只有最后一层会确定this指向的是什么，不管有多少层，在判断this的时候，我们只关注最后一层，即此处的friend。
    
    > 隐式绑定有一个大陷阱，绑定很容易丢失(或者说容易给我们造成误导，我们以为this指向的是什么，但是实际上并非如此).
    ```
      function sayHi(){
          console.log('Hello,', this.name);
      }
      var person1 = {
          name: 'YvetteLau',
          sayHi: function(){
              setTimeout(function(){
                  console.log('Hello,',this.name);
              })
          }
      }
      var person2 = {
          name: 'Christina',
          sayHi: sayHi
      }
      var name='Wiliam';
      person1.sayHi(); // 'Hello, Wiliam'
      setTimeout(person2.sayHi,100); // 'Hello, Wiliam'
      setTimeout(function(){
          person2.sayHi();
      },200); // 'Hello, Christina'
    ```
    - 第一条   setTimeout的回调函数在执行栈中执行，this使用的是默认绑定，非严格模式下，执行的是全局对象
    - 第二条   相当于将person2.sayHi的函数地址赋值给setTimeout的回调函数并在执行栈中执行,this使用的是默认绑定，非严格模式下，执行的是全局对象
    - 第三条   相当于在执行站中执行回调函数, 执行回调函数中的语句, 使用的是隐式绑定，因此这是this指向的是person2

    ### 显示绑定
    定义: 通过call,apply,bind的方式，显式的指定this所指向的对象。

    call,apply和bind的第一个参数，就是对应函数的this所指向的对象。call和apply的作用一样，只是传参方式不同。**call和apply都会执行对应的函数，而bind方法不会**。
    ```
      function sayHi(){
          console.log('Hello,', this.name);
      }
      var person = {
          name: 'YvetteLau',
          sayHi: sayHi
      }
      name = 'Wiliam';
      var Hi = person.sayHi;
      Hi.call(person); // 'Hello,YvetteLau'
      Hi.apply(person); // 'Hello,YvetteLau'
      // Hi函数的this指向绑定给了person
    ```
    那么，使用了硬绑定，是不是意味着不会出现隐式绑定所遇到的绑定丢失呢？显然不是这样的，不信，继续往下看。
    ```
      function sayHi(){
          console.log('Hello,', this.name);
      }
      var person = {
          name: 'YvetteLau',
          sayHi: sayHi
      }
      name = 'Wiliam';
      var Hi = function(fn) {
          fn();
      }
      Hi.call(person, person.sayHi); // 'Hello, Wiliam'
      // 因为Hi的this指向绑定给了person, 但是Hi的形参调用是默认绑定给window对象
      // 现在，我们希望绑定不会丢失，要怎么做？很简单，调用fn的时候，也给它硬绑定。
      var Hi = function(fn) {
          fn.call(this);
      }
      Hi.call(person, person.sayHi);
    ```

    ### new绑定
    javaScript和Ｃ＋＋不一样，并没有类，在javaScript中，构造函数只是使用new操作符时被调用的函数，这些函数和普通的函数并没有什么不同，它不属于某个类，也不可能实例化出一个类。任何一个函数都可以使用new来调用，因此其实并不存在构造函数，而只有对于函数的“构造调用”。
    > 使用new来调用函数，会自动执行下面的操作：
    1. 创建一个空对象，构造函数中的this指向这个空对象
    2. 这个新对象被执行 [[原型]] 连接
    3. 执行构造函数方法，属性和方法被添加到this引用的对象中
    4. 如果构造函数中没有返回其它对象，那么返回this，即创建的这个的新对象，否则，返回构造函数中返回的对象。
    ```
      function _new() {
        let target = {}; //创建的新对象
        //第一个参数是构造函数
        let [constructor, ...args] = [...arguments];
        //执行[[原型]]连接;target 是 constructor 的实例
        target.__proto__ = constructor.prototype;
        //执行构造函数，将属性或方法添加到创建的空对象上
        let result = constructor.apply(target, args);
        if (result && (typeof (result) == "object" || typeof (result) == "function")) {
            //如果构造函数执行的结构返回的是一个对象，那么返回这个对象
            return result;
        }
        //如果构造函数返回的不是一个对象，返回创建的新对象
        return target;
      }
    ```
    因此，我们使用new来调用函数的时候，就会新对象绑定到这个函数的this上。
    ```
      function sayHi(name){
          this.name = name;
        
      }
      var Hi = new sayHi('Yevtte');
      console.log('Hello,', Hi.name); // 'Hello, Yevtte'
    ```

## this的绑定优先级
我们知道了this有四种绑定规则，但是如果同时应用了多种规则，怎么办？
显然，我们需要了解哪一种绑定方式的优先级更高，这四种绑定的优先级为:
new绑定 > 显式绑定 > 隐式绑定 > 默认绑定

## 绑定例外
如果我们将null或者是undefined作为this的绑定对象传入call、apply或者是bind,这些值在调用时会被忽略，实际应用的是默认绑定规则。
```
  var foo = {
    name: 'Selina'
  }
  var name = 'Chirs';
  function bar() {
      console.log(this.name);
  }
  bar.call(null); //Chirs 
```
输出的结果是 Chirs，因为这时实际应用的是默认绑定规则。

## 箭头函数
箭头函数是ES6中新增的，它和普通函数有一些区别，**箭头函数没有自己的this，它的this继承于上层作用域中的this**。箭头函数在使用时，需要注意以下几点:
1. 函数体内的this对象，继承的是外层代码块的this。
2. 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
3. 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
  ```
    let bar = () => {
        console.log(arguments);
    }
    bar(55); // [callee: ƒ, Symbol(Symbol.iterator): ƒ]
    let bar2 = () => {
        console.log(arguments);
    }
    bar2(55); // [55, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  ```
4. 不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
5. 箭头函数没有自己的this，所以不能用call()、apply()、bind()这些方法去改变this的指向.
OK，我们来看看箭头函数的this是什么？
```
  var obj = {
    hi: function(){
        console.log(this);
        return ()=>{
            console.log(this);
        }
    },
    sayHi: function(){
        return function() {
            console.log(this);
            return ()=>{
                console.log(this);
            }
        }
    },
    say: ()=>{
        console.log(this);
    }
  }
  let hi = obj.hi();  //输出obj对象
  hi();               //输出obj对象
  let sayHi = obj.sayHi();
  let fun1 = sayHi(); //输出window
  fun1();             //输出window
  obj.say();          //输出window
```
分析:
- obj.hi执行,进行隐式绑定this指向obj, 
- hi执行, 由于箭头函数的继承性, this指向继承自上级this指向: obj 
- obj.sayHi执行返回的函数赋值给sayHi, sayHi执行返回箭头函数, 进行默认绑定this指向window, 将返回函数赋值给fun1并执行, this指向继承自上级this指向: window
- obj.say箭头函数执行, this指向继承自上级this指向obj中是不存在this,只能往上找: window

## 小知识点
1. 严格模式下指向window的this都是指向undefined
2. 自执行函数, 或直接调用函数的this指向window
3. 通过call, applay, bind改变this指向(非箭头函数的常规函数): 
```
  obj.fun.call() //  this => window
  obj.fun.call(undefined) //  this => window
  obj.fun.call(undefined) //  this => window
  obj.fun.call(null) // this => window
  obj.fun.call('1') // this => String {'1'}
  obj.fun.call(10) // this => Number {10}
  obj.fun.call(true) // this => Boolean {true}
  obj.fun.call([1, 2]) // this => [1, 2]
  obj.fun.call(对象) // this => 对象
```
4. this绑定优先级 new绑定 > 显式绑定 > 隐式绑定 > 默认绑定
5. 由于箭头函数本身没有this, 所以对箭头函数进行显示绑定不生效
6. 箭头函数的this继承于上层此法作用域中的this

## 改变this指向(call, appplay, bind)
- 语法：
```
  fun.call(thisArg, param1, param2, ...)
  fun.apply(thisArg, [param1,param2,...])
  fun.bind(thisArg, param1, param2, ...)
```
- 返回值：call, apply立即执行函数, bind是返回已经改变this指向的函数
- 参数: 
1. `thisArg(可选):`
    1. fun的this指向thisArg对象
    2. 非严格模式下：thisArg指定为null，undefined，fun中的this指向window对象.
    3. 严格模式下：fun的this为undefined
    4. 值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象，如 String、Number、Boolean
2. param1,param2(可选): 传给fun的参数
    1. 如果param不传或为 null/undefined，则表示不需要传入任何参数.
    2. apply第二个参数为数组，数组内的值为传给fun的参数。
   
`调用call/apply/bind的必须是个函数
call、apply和bind是挂在Function对象上的三个方法,只有函数才有这些方法。`
```
  Object.prototype.toString.call(data)
```
## 作用：改变函数执行时的this指向，目前所有关于它们的运用，都是基于这一点来进行的。
## 区别
1. call与apply的唯一区别
   - apply是第2个参数，这个参数是一个数组：传给fun参数都写在数组中。
   - call从第2~n的参数都是传给fun的。
2. call/apply与bind的区别
   - call/apply改变了函数的this上下文后马上执行该函数
   - bind则是返回改变了上下文后的函数,不执行该函数
3. 返回值
   - call/apply 返回fun的执行结果
   - bind返回fun的拷贝，并指定了fun的this指向，保存了fun的参数。

## 手写call/apply、bind：
### call

