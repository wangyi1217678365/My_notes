### redux是什么
> redux是一个专门用于做状态管理的js库（不是react插件库）。
> 它可以用在react，angular，vue等项目中，但基本与react配合使用。
> 作用：集中式管理react应用中多个组件共享的状态。
### 什么情况下使用redux
1. 某个组件的状态，需要其它组件可以随时拿到（共享）
2. 一个组件需要改变另一个组件的状态（通信）。
3. 总体原则：能不用就不用，如果不用比较吃力才考虑使用。
### redux工作流程