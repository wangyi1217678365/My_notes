### 数据类型
#### 基本类型
- **String (字符串)**
  - 写法(引号不属于字符串的一部分)
    - 单引号 'hi'
    - 双引号 "hi"
    - 反引号 ``
  - 转义
    - ' —— ' 单引号
    - " —— " 双引号
    - \n —— 换行
    - \r —— 回车
    - \t —— tab 制表符
    - \ —— \斜杠
    - \uFFFF —— 对应的Unicode 字符
    - \xFF —— 前 256 个Unicode 字符
    - 多行字符串：外面用反引号
  - 字符串长度
    - string.length —— 即可看见字符串string的长度 例如：'\n\r\t'.length //值为3
    - 通过下标可以读取字符（类似数组） 例如：let s='hello';s[0] //值为"h"
  - base64转码
    - window.btoa(字符串)—— 正常字符串转为Base64编码的字符串
    - window.atob(字符串) —— Base64编码的字符串转为原来的字符串 


- **Boolean (布尔值)**
  - 五个 falsy 值
    1. undefined
    2. null
    3. 0
    4. NaN
    5. ''(空字符串)
  - 这五个 falsy 值代表假的。（还有false）剩下的都为true


- **Number (数字)**
  - 数字的特殊值有                   
    -  0, +0, -0 
    -  无穷大 Infinity、正无穷大+Infinity、负无穷大-Infinity。
    -  无法表达的数字 —— NaN，但是输入NaN===NaN,会出现false
  - 数字的存储形式(数字是用64位浮点数的形式存储的。（二进制） )
    - 二进制无法表示无限不循环小数.导致出现浮点数
    - 解决方法:
      1. 引用类库:Math.js, decimal.js, big.js
      2. 封装处理函数 https://juejin.cn/post/6844903903071322119


- **symbol (符号)**
- **null (空值), undefined (未定义)**
  - 声明变量但是没有赋值，默认值是undefined，不是null。
  - 函数没有return，那么默认return undefined，而不是 null。
  - 习惯上，非对象的空值写为undefined，把对象的空值写为 null。
> string 、number 、boolean 和 symbol 这四种类型统称为原始类型（Primitive），表示不能再细分下去的基本类型；symbol 表示独一无二的值，通过 Symbol 函数调用生成，由于生成的 symbol 值为原始类型，所以 Symbol 函数不能使用 new 调用；null 和 undefined 通常被认为是特殊值，这两种类型的值唯一，就是其本身。
#### 引用类型(复杂数据类型)
> 存储数据的空间可以存储任意数据类型
- **Array**
  - 存储空间里的值是有序的.
- **Object**
  - 存储空间里的值是无序的,以键值对key: value的形式存储
---
### 类型转换
- **强制转换**
  - toString(): 方法返回一个表示该对象的字符串。
  ```
    var num = new Number(123);
    console.log(num.toString()); //'123'

    var date = new Date();
    console.log(date.toString()); //"Fri Nov 19 2021 23:55:04 GMT+0800 (中国标准时间)"

    var bool = new Boolean('123');
    console.log(bool.toString()); //'true'

    var obj = new Object({a: 5})
    console.log(obj.toString()); //"[object Object]"

    var obj = new Array(1, 2)
    console.log(obj.toString()); //"1, 2"
  ```
  - valueOf(): 方法返回指定对象的原始值。
  ```
    var str = new String('123');
    var str1 = new String(123);
    console.log(str.valueOf(), str1.valueOf()); //'123' '123'

    var num = new Number(123);
    console.log(num.valueOf()); //123

    var date = new Date();
    console.log(date.valueOf()); //1526990889729

    var bool = new Boolean('123');
    console.log(bool.valueOf()); //true

    var obj = new Object({a: 5})
    console.log(obj.valueOf()); //{a: 5}
  ```
  - 转数字
    - Number()
    ```
      console.log(Number(null)); //0
      console.log(Number(false)); //0
      console.log(Number(true)); //1
      console.log(Number(undefined)); //NaN
      console.log(Number('11')); //11
      console.log(Number('11.1')); //11.1
      console.log(Number('11.w')); //NaN
    ```
    - parseInt(): 转整数 从第一位开始识别转化
    ```
      console.log(parseInt('11')); //11
      console.log(parseInt(false), parseInt(undefined), parseInt(null), parseInt('ww')); //NaN
      console.log(parseInt('1w')); //1
      console.log(parseInt('1.1')); //1
    ```
    - paresFloat(): 转化规则同paresInt() 区别在于可识别一个小数点
    ```
      console.log(parseInt('1.1')); //1.1
    ```
  - 转字符串:String()
  ```
    String(null)                 // 'null'
    String(undefined)            // 'undefined'
    String(true)                 // 'true'
    String(1)                    // '1'
    String(-1)                   // '-1'
    String(0)                    // '0'
    String(-0)                   // '0'
    String(Math.pow(1000,10))    // '1e+30'
    String(Infinity)             // 'Infinity'
    String(-Infinity)            // '-Infinity'
    String({})                   // '[object Object]'
    String([1,[2,3]])            // '1,2,3'
    String(['koala',1])          //koala,1
  ```
  - 转布尔类型:Boolean()
  > 除了 true\undefined\null\-0\0或+0\NaN\''（空字符串）值转换结果为 false，其他全部为true
- **隐式转换**
  - 字符串加法运算: 当一个值为字符串，另一个值为非字符串，则后者转为字符串。
  - 自动转换为Number类型
    - 有加法运算符，但是无String类型的时候，都会优先转换为Number类型, 除了加法运算符，其他运算符都会把运算自动转成数值。注意: null转为数值时为0，而undefined转为数值时为NaN。
    - 一元运算符
    ```
      +'11' // 11
      -'11' // 11
      +true // 1
      -false // 0
    ```
    - 逻辑运算符
      - 会将运算符两边的数据类型优先转换为数字类型进行比较
---
### 数据类型判断
- typeof: 通过 typeof操作符来判断一个值属于哪种基本类型。
```
  typeof 'seymoe'    // 'string'
  typeof true        // 'boolean'
  typeof 10          // 'number'
  typeof Symbol()    // 'symbol'
  typeof null        // 'object' 无法判定是否为 null
  typeof undefined   // 'undefined'

  typeof {}           // 'object'
  typeof []           // 'object'
  typeof(() => {})    // 'function'
  typeof new Date()   // 'object'
```
  缺点: 无法识别null、对象除了function外的子类型
- instanceof: 通过 instanceof 操作符也可以对对象类型进行判定，其原理就是测试构造函数的 prototype 是否出现在被检测对象的原型链上。
```
  [] instanceof Array            // true
  ({}) instanceof Object         // true
  (()=>{}) instanceof Function   // true
```
 缺点: instanceof 仍然无法判断优雅的判断一个值到底属于数组还是普通对象。
- Object.prototype.toString(): 可以说是判定 JavaScript 中数据类型的终极解决方法了
```
  Object.prototype.toString.call({})              // '[object Object]'
  Object.prototype.toString.call([])              // '[object Array]'
  Object.prototype.toString.call(() => {})        // '[object Function]'
  Object.prototype.toString.call('seymoe')        // '[object String]'
  Object.prototype.toString.call(1)               // '[object Number]'
  Object.prototype.toString.call(true)            // '[object Boolean]'
  Object.prototype.toString.call(Symbol())        // '[object Symbol]'
  Object.prototype.toString.call(null)            // '[object Null]'
  Object.prototype.toString.call(undefined)       // '[object Undefined]'

  Object.prototype.toString.call(new Date())      // '[object Date]'
  Object.prototype.toString.call(Math)            // '[object Math]'
  Object.prototype.toString.call(new Set())       // '[object Set]'
  Object.prototype.toString.call(new WeakSet())   // '[object WeakSet]'
  Object.prototype.toString.call(new Map())       // '[object Map]'
  Object.prototype.toString.call(new WeakMap())   // '[object WeakMap]'
```
---
### 变量
- 变量声明
  1. var: var在方法中定义的变量是局部变量，在全局作用域定义的变量是全局变量，var存在变量提升。
  2. let: 声明的变量是可变的
  3. const: 声明的变量是不可变的, 切声明必赋值
- 变量存储: 变量在内存中的具体存储形式
  - 栈内存: 基本类型是保存在栈内存中的简单数据段，它们的值都有固定的大小，保存在栈空间，通过按值访问
  - 堆内存: 引用类型是保存在堆内存中的对象，值大小不固定，栈内存中存放的该对象的访问地址指向堆内存中的对象，JavaScript不允许直接访问堆内存中的位置，因此操作对象时，实际操作对象的引用
  ```
    let a1 = 0; // 栈内存
    let a2 = "this is string" // 栈内存
    let a3 = null; // 栈内存
    let b = { x: 10 }; // 变量b存在于栈中，{ x: 10 }作为对象存在于堆中
    let c = [1, 2, 3]; // 变量c存在于栈中，[1, 2, 3]作为对象存在于堆中
  ```
  ![变量存储](https://upload-images.jianshu.io/upload_images/12665637-b7878064411df56a.png?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)
  当我们要访问堆内存中的引用数据类型时: 
  1. 从栈中获取该对象的地址引用
  2. 再从堆内存中取得我们需要的数据
---
### 条件分支语句
---
### 循环
---
### 运算符
---
### 函数
1. 定义函数：
 ```
   function fun() {} // 声明式
   var fun = function () {} // 赋值式
 ```
2. 函数的调用
   1. 声明式的调用  可以在函数定义之前调用
   2. 赋值式的调用  只能在函数定义之后调用
   3. 为什么会造成时机的不同([详见预解析](#预解析))
3. 函数的两个阶段
   1. 定义阶段
      1. 开空间
      2. 放代码（不解析变量)
      3. 把空间地址给函数名称（只有声明式定义函数会有这一步）
   2. 调用阶段
      1. 根据函数名称在栈内存中找到相对应的存储在堆内存中的函数地址
      2. 行参赋值
      3. 预解析
      4. 执行函数体内的代码（解析变量）
4. 函数的参数
   1. 行参： 在函数内部使用的变量，只能在函数内部使用
   ```
    function fun(a) {console.log(a)} // 这里的a就是行参
   ```
   2. 实参： 在函数调用阶段传入的参数
   ```
    fun(10) // 这里的10就是实参
   ```
   3. arguments在函数内部获取函数的所有实参，可以不用行参接收，在函数内部使用，所有实参的集合。（arguments是一个伪数组，不是一个真正的数组）
   ```
    function fun(a) {
        console.log(arguments) // [1, 3, 5, -1]
    }
    fun(1, 3, 5, -1)
   ```
5. 函数的return
   1. return表示返回，只能在函数中使用
   2. return可以阻断代码执行
   3. return可以返回一个值
   ```
    function fun() {return 2}   console.log(fun()) === 2 
    function fun() {return}   console.log(fun()) === undefined
    function fun() {}   console.log(fun()) === undefined
   ``` 
---

### 预解析
> 当浏览器加载html页面的时候，首先会提供一个供全局JavaScript代码执行的环境，称之为全局作用域。在当前作用域中，JavaScript代码执行之前，浏览器首先会默认的把所有带var和function声明的变量进行提前的声明或者定义。
- 声明和定义
  - 声明：var num; 告诉浏览器在全局作用域中有一个num变量了，如果一个变量只是声明了，但是没有赋值，默认值是undefined。
  - 定义：num = 12; 定义就是给变量进行赋值。
- var声明的变量和function声明的函数在预解析的区别
  - var声明的变量和function声明的函数在预解析的时候有区别，var声明的变量在预解析的时候只是提前的声明，function声明的函数在预解析的时候会提前声明并且会同时定义。也就是说var声明的变量和function声明的函数的区别是在声明的同时有没同时进行定义。
- 预解析只发生在当前的作用域下
  - 程序最开始的时候，只对window下的变量和函数进行预解析，只有函数执行的时候才会对函数中的变量和函数进行预解析。
```
    console.log(num); // undefined
    var num = 24;
    console.log(num); // 24

    func(100 , 200);  // 300
    function func(num1 , num2) {
        var total = num1 + num2;
        console.log(total);
    }
```
第一次输出num的时候，由于预解析的原因，只声明了还没有定义，所以会输出undefined；第二次输出num的时候，已经定义了，所以输出24。
由于函数的声明和定义是同时进行的，所以func()虽然是在func函数定义声明处之前调用的，但是依然可以正常的调用，会正常输出300。

---

### 作用域
- 函数里面的作用域为私有作用域，window所在的作用域称为全局作用域；
- 在全局作用域下声明的变量是全局变量；
- 在“私有作用域中声明的变量”和“函数的形参”都是私有变量；
> 在私有作用域中，代码执行的时候，遇到了一个变量，首先需要确定它是否为私有变量，如果是私有变量，那么和外面的任何东西都没有关系，如果不是私有的，则往当前作用域的上级作用域进行查找，如果上级作用域也没有则继续查找，一直查找到window为止，这就是作用域链。  
              
**作用:**
1. 变量隔离：我们可以把作用域理解为一种空间策略，每个函数拥有自己的私有作用域，它对变量进行了隔离，控制了变量的可访问性，因此不同的作用域可以具有相同名称的变量而不冲突。
- 当函数执行的时候，首先会形成一个新的私有作用域，然后按照如下的步骤执行：
  1. 如果有形参，先给形参赋值；
---

### 闭包


---
### 常用的api