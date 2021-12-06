# 三大属性 State、Props、Ref

## State 运用

## Props 运用

## Ref 运用
### 字符串式的ref（不推荐）
```
<div ref='target'></div>
this.refs.target
```

### 回调形式的ref
```
<div ref={e => this.target = e}></div>
this.target
```
> react识别出回调式ref后会自动执行并抛出当前标识的dom

> 回调次数：如果回调函数是已内联函数的形式定义的，在更新阶段它会被执行两次。第一次返回null，第二次返回当前标识的dom。这是因为每次渲染时会创建一个新的函数实例，所以react清空旧的ref并设置新的。通过将ref的回调函数定义成class的绑定函数的方式可以避免上述问题，但是大多情况下它是无关紧要的。

```
<div ref={this.getTarget}></div>
getTarget = (e) => {
    console.log(e)
}
``` 

### creatRef
> React.creatRef调用后可以返回一个容器，该容器可以存储被ref标识的节点

```
<div ref={this.getTarget}></div>
getTarget = react.creatRef()
console.log(this.getTarget.current)
```     
