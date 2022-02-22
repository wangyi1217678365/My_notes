# ES6特性：
## 一. 扩展运算符（展开运算符）
`特性：可以在函数调用/数组构造时，将数组表达式或者string在语法层面上展开。还可以在构造对象时，将对象表达式按照key-value的方式展开`    
- 功能：      
    1. 展开数组|对象|字符串|Set集合|Map字典：     
    ```
        // 数组:
        var  arr  =  [1, 2, 3]
        console.log(...arr)   // 1 2 3
        // 对象：
        var obj = { a: 5, b: 9 }
        console.log({...obj})   // { a: 5, b: 9 }
        // 字符串：
        console.log(...'5 5 5')   // '5' ' ' '5' ' ' '5' 
        // Set集合：
        var set = new Set([1, 2, 3])
        console.log(...set)  // 1 2 3
        // Map字典：
        var map = new Map([[1, 2], [3, 4]])
        console.log(...map)   // [1, 2] [3, 4]
    ```
    2.  拷贝（深拷贝最外层数据结构）
    ```
        var arr = [1, 2, 3, [8, 9]]
        var arr2 = [...arr]
        arr2[0] = 10
        arr2[3][0] = 10
        arr[3][0]  ===  arr2[3][0]  //  true
        var obj = {
            a: 10,
            b: {a: 100}
        }
        var obj2 = {...obj}
        obj2.a = 100
        obj2.b.a = 10
        obj.b.a  ===  obj2.b.a  //  truep
    ```
    3.  合并数组|对象|字符串|Set集合|Map字典: 只要可以被展开的都可以合并
    ```
        // 数组
        var arr = [1, 2, 3]
        var arr2 = [...arr, 1, 2, 3]
        console.log(arr2)  // [1, 2, 3, 1, 2, 3]
        // 对象
        var obj = {a: 10, b: 20}
        var obj2 = {...obj, a: 100, c: 5}
        console.log(obj2)  // {a: 100, b: 20, c: 5}
        ....
    ```

## 二. Set: 用来生成Set的数据结构，是一种集合数据结构
- 特点:    
    1. 它里面每一项的值是唯一的，没有重复的值
    2. 参数可以是数组及伪数组等有序的数据结构
    3. size用来表示数据个数
    4. 你可以按照插入的顺序迭代它的元素
- API:    
    1. 添加一项:  add(value)
    2. 删除某一项:  delete(value)
    3. 检测是否存在某一项返回布尔值:  has(value)
    4. 清空集合:  clear(value)
- 使用场景: 
    1. 去重：传统ES5去重，创建空的数组，便利目标数组利用数组的indexOf方法查找,性能比较差，而其无法去重NaN。使用Set去重之后可以利用Array.from(set数据)或扩展运算符[...set数据]用来将Set数据结构转成数组。
- 缺点:
    1. 无法比较0, -0, +0

## 三. Map: 用来生成map的数据结构，是一种字典数据结构
- 特点:    
    1. 它里面每一项的值是唯一的，没有重复的值
    2. 参数 var m = new Map([['a', 1]])  
    3. size用来表示数据个数
    4. 键值对的集合，但是“键”的范围不限于字符串
- API:     
    1. 同set除了添加一项
    2. 添加一项:  set(value)   set({a:1}, 5)
    3. 通过键值查找特定的数值并返回:  get(key)
- 使用场景: 
    1. 去重：传统ES5去重，创建空的对象，利用对象的属性值唯一用来判断便利数组的每一项是否已存在对象当中。 由于对象的key值会转化字符串会导致不同类型的值当成相同的属性被过滤掉。使用Map建立映射关系。去重之后可以利用Array.from(set数据)或扩展运算符[...map数据]用来将Map数据结构转成数组。
                
                
## 四. 箭头函数
- 语法: () => {}
  1. 如果形参只有一个可以省略小括号
   ```
    let fun = function (value) {}
    let fun2 = value => {}
   ```
   2. 如果花括号只有一句执行语句, 自动执行return并且可以省略花括号
   ```
    let fun2 = function (value) {
    return value;
    }
    let fun = value => value
   ```
   3. 返回一个对象
   ```
    let func = (value, num) => ({total: 10})
   ```
- 特性:
  1. 无法被new构造调用
  2. 内部没有arguments对象
  3. 本身没有this指向, 无法通过硬绑定(call, applay, bind)来改变它的this指向, 它的this指向继承于上级词法作用域中的this, 遵循变量访问规则.
   
## 五. 解构赋值
`主要用来解决访问链过长`
### 对象的解构赋值
1. 同名变量赋值
```
let details = {
    firstName:'Code',
    lastName:'Burst',
    age:22
}
const {firstName,age} = details;
console.log(firstName); // Code
console.log(age);       // 22
```
2. 非同名变量赋值
```
const person = {
  name: 'jsPool',
  country: 'China'
};
const {name:fullname,country:place} = person;
console.log(fullname); // jsPool
console.log(place); // China
```
3. 默认值
```
const person = {
  name: 'jsPool',
  country: 'China',
  sexual:undefined
};
let {age = 20,sexual:sex = 'male'} = person;
console.log(age); // 20
console.log(sex); // male
```
4. 嵌套对象的解构赋值
```
const student = {
    name:'jsPool',
    age:20,
    scores:{
        math:95,
        chinese:98,
        english:93
    }
}
function showScore(student){
    const { name,scores:{ math = 0,chinese = 0, english = 0}} = student;
    console.log('学生名:' + name);
    console.log('数学成绩:' + math);
    console.log('语文成绩:' + chinese);
    console.log('英语成绩:' + english);
}
showScore(student)
```
5. 不定元素
```
const person = {
  name: 'jsPool',
  country: 'China',
  sexual:undefined
};
let {name, ...orther} = person;
console.log(name); // 'jsPool'
console.log(orther); // {country: 'China', sexual: undefined}
```

### 数组的解构赋值
1. 变量赋值
```
let list = [221,'Baker Street','London'];
let [houseNo,,city] = list; console.log(houseNo,city);// 221 , London
```
2. 默认值
```
let list = [221,'Baker Street'];
let [houseNo,street,city = 'BJ'] = list;
console.log(houseNo,street,city);// 221 , Baker Street , BJ
```
3. 嵌套数组的解构赋值
```
let colors = [ 'red' , [ 'green' , 'yellow' ] , 'blue' ];
let [ firstColor , [ secondColor ] ] = colors;
console.log(firstColor,secondColor); // red , green
```
4. 不定元素
```
let colors = ['red','green','blue'];
let [firstColor,...otherColors] = colors;
console.log(firstColor);  // red
console.log(otherColors); // ['green','blue']
```