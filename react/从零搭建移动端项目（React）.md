#### 从零搭建移动端项目（React）

#####  创建项目

```react
npx create-react-app myown
```

##### 目录结构后续补充

##### 项目架构公共部分

- 公共config

  ```javascript
  let config = {
    mode: 'uat2',
    apiServer: {
      test: 'http://192.168.99.145/api',
      uat2: 'http://uat-appweb.tijianbao.com/api', // uat2 环境接口
      uat: 'https://uat-appweb.tijianbao.com/api', // UAT 环境接口
      pre: 'https://pre-appweb.tijianbao.com/api', // 预生产 环境接口
      prod: 'https://appweb.tijianbao.com/api' // 正式 环境接口
    },
   }
   export default config
  ```

- ***http请求封装***

  1. fetchData:将接口传的参数进行统一处理

     ```javascript
     const fetchData = (uri, data = {}, type = 'GET') => {
       axios.defaults.withCredentials = true;
       type = type.toUpperCase();
       let url = config.apiServer[window.pre_host] + uri;
       let o = {
         method: type,
         url,
         data
       }
       if(type === 'GET') {
         o.params = data
       }
       return axios(o);
     };
     ```

     2. axios:对axios请求以及响应做进一步处理

        - 先是判断 传的参数是不是 字符串格式 如果是字符串格式 将转化成 对象格式 JSON.parse
        
        - 在判断是否有 showLoading 这个字段 如果有 则调取 加载动画 方法 然后再将其 字段 删除
        
        - 对于多个接口 都需要 loading 动画是 的处理
        
          ```javascript
          // 先是声明一个 存放 需要loading 动画的 url 数组
          let loadingRequireUrl = [];
          // 根据 showloading 是否展示 来存放
          loadingRequireUri.push(config.url);
          // response 拦截器里
          // 先是获取返回来的  请求地址 是否在 存放loading的数组里
          let loadingRequireUriIndexOf = loadingRequireUri.indexOf(
             response.config.url
          );
          // 如果有 则将数组里删除掉
          // 最后 如果等于 0 则调取 隐藏动画方法
          if (loadingRequireUriIndexOf > -1) {
               loadingRequireUri.splice(loadingRequireUriIndexOf, 1);
               if (loadingRequireUri.length === 0) {
                  store.dispatch({
                    type: STOP_LOADING
               });
             }
          }
          ```
        
        - 对axios 进行补充
        
        - [同源策略](https://blog.csdn.net/weixin_40686603/article/details/105265565?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-10.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-10.control)
        
          1. ```javascript
             axios.defaults.withCredentials = true; //允许跨域携带cookie信息* 
             ```
        
          2. 取消接口方法
        
             ```javascript
             // 保存 需要取消的接口
             let cancelArr = {}
             // 接口传参数 是否传 取消接口的标识 cancelToken 
             // 处理需要取消请求的接口
                 if (reqData.cancelToken) {
                   // 循环取消当前保留的接口
                   for (let prop in cancelArr) {
                     cancelArr[prop]()
                   }
                   // 对该接口注册取消事件
                   config.cancelToken = new axios.CancelToken(cancel => {
                     cancelArr[config.url] = cancel
                   })
                 }
              // 接口回来删除保存在cancelArr对象里的属性
              if (response.config.cancelToken) {
                 delete cancelArr[response.config.url]
              }
             ```

- ***公共方法***

  1. 根据端口来判断什么环境

     ```javascript
     export function getPreHost () {
       let predefinedHost = ['test', 'uat', 'uat2', 'pre', 'prod']
       let searchPreHost = window.location.hostname;
       console.log('环境配置', config)
       let preHost =
         searchPreHost && predefinedHost.includes(searchPreHost[0])
           ? searchPreHost[0]
           : 'prod' // 赋值，为 null 则赋值 prod
       if (/^localhost|^127\.0\.0\.1|^192\.168\.|^10\.108\./.test(window.location.hostname)) {
         preHost = config.mode // 本机开发模拟
       }
       return preHost
     }
     
     ```

  2. 初始化过程赋值

     ```javascript
     window.pre_host = getPreHost() // 取得 host 前缀（test/uat/pre/prod）
     ```

- ***头部组件封装***

  1. 封装思想:是否需要左边返回键，中间区域是文字还是组件 右边区域是组件还是不展示

  2. 背景是否需要渐变还是白色背景，头部组件是否需要border-bottom

     ```javascript
     // 背景渐变
     const scrollHeight = this.props.scrollHeight
     const linearHeight = this.props.linearHeight || 44
     style={{ backgroundColor: `rgba(255,255,255,${scrollHeight / linearHeight})` }}
     // 字体渐变
      let scale = 1 - (scrollHeight / linearHeight > 1 ? 1 : scrollHeight / linearHeight)
      return (
        <span style={{ color: `rgba(${255 * scale}, ${255 * scale}, ${255 * scale}, 1)`, 
      )
     ```

- ***router***

  - 项目里包含的一些路由文件

  - ```react
    import { withRouter, Route } from 'react-router-dom'
    // react-router-dom 
    // withRouter: 作用是将一个组件包裹进Route里面, 然后react-router的三个对象	     history, location, match就会被放进这个组件的props属性中.
    // Route 路由跳转相关组件
    import Loadable from 'react-loadable'
    // Loadable 对组件进行异步加载处理
    import CacheRoute, { CacheSwith } from 'react-router-cache-route'
    // cacheRouter cacheSwith 相当于 keep-alive 跳转页面前 缓存页面
    // CacheRoute 使用 CacheRoute 的组件将会得到一个名为 cacheLifecycles 的属性，里面包含两个额外生命周期的注入函数 didCache 和 didRecover，分别用在组件 被缓存 和 被恢复 时
    ```


- ***TabBar***   待优化

  - 该组件中需要了解得知识点: createPortal 

  - <a href="https://www.jianshu.com/u/1f5ac0cf6a8b" target="_blank">React Portal 传送门</a>

  - 遇到的问题 ：

    1. ```javascript
       <img src=[object Module] />
       // 解决办法
       // webpack.config.js  添加esMoudle 为 false
        {
          test: [/\.bmp$/, /\.gif$/, /\.jpe?g$/, /\.png$/],
          loader: require.resolve('url-loader'),
          options: {
             limit: imageInlineSizeLimit,
             name: 'static/media/[name].[hash:8].[ext]',
             esModule: false
           }
         },
       ```

    2.  a 标签背景默认有蓝色背景 解决办法：

       ```css
       
       a {
         -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
        -webkit-user-select: none;
        -moz-user-focus: none;
        -moz-user-select: none;
         background-color: transparent;
       }
       ```
    
    3. img 属性  srcset： 用于浏览器根据宽、高和像素密度来加载相应的图片资源。 
    
       ```html
       srcSet={`${require('@/static/images/tabbar/tabbar_home_normal@2x.png')} 2x, ${require('@/static/images/tabbar/tabbar_home_normal@3x.png')} 3x`}
       ```
    
    4. 用createPortal创建的元素 在 react 生命周期的 卸载周期要将其 移除，不然会在页面没切换路由一次就添加一次。
    
       ```javascript
       // 创建DOM 节点
       const doc = window.document;
       this.node = doc.createElement('div')
       doc.body.appendChild(this.node)
       // 在卸载周期移除
       componentWillUnmount () {
        window.document.body.removeChild(this.node);
       }
       ```
    
  

#####  数据仓库 Store

- 在这之前先了解 数据仓库 一些生态圈

- [Redux](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

- [Rdux中间件异步操作](ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html)

- [react-redux](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)

- [redux-persit使用](https://www.cnblogs.com/ljwk/p/9605444.html)

- [redux-persit进阶](https://www.cnblogs.com/smart-girl/p/10875749.html)

- [踩坑](https://blog.csdn.net/zccz14/article/details/51619587?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&dist_request_id=1619751900925_85032&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)

  1. createStore : 

  2. ```tex
     store.dispatch`方法会触发 Reducer 的自动执行
     Reducer 函数最重要的特征是，它是一个纯函数。也就是说，只要是同样的输入，必定得到同样的输出。
     
     store.subscribe方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。
     
     store.subscribe方法返回一个函数，调用这个函数就可以解除监听
     let unsubscribe = store.subscribe(() =>
       console.log(store.getState())
     );
     unsubscribe();
     
     Redux 提供了一个combineReducers方法，用于 Reducer 的拆分
     ```

  3. 

     1.  

     - 创建一个 Redux [store](http://www.redux.org.cn/docs/api/Store.html) 来以存放应用中所有的 state。
     - 应用中应有且仅有一个 store。
     - 接受两个参数 第一个参数是

##### 骨架屏

- 先是引用 react-content-loader 骨架屏

- 然后根据自身情况 建造相应功能的骨架屏

- 最后根据页面组合相应骨架屏

- 示例：

  ```javascript
  // 先是创建不同样式的骨架屏
  import React from 'react';
  import ContentLoader from 'react-content-loader';
  export const IndexSwiperLoader = props => (
    <ContentLoader
      speed={2}
      width={375}
      height={160}
      primaryColor="#f3f3f3"
      secondaryColor="#ecebeb"
      {...props}
    >
      <rect x="16" y="0" rx="0" ry="0" width="105" height="80" />
      <rect x="133" y="0" rx="0" ry="0" width="226" height="22" />
      <rect x="133" y="28" rx="0" ry="0" width="226" height="34" />
      <rect x="135" y="77" rx="0" ry="0" width="69" height="23" />
      <rect x="136" y="107" rx="0" ry="0" width="51" height="11" />
      <rect x="281" y="78" rx="0" ry="0" width="77" height="18" />
    </ContentLoader>
  )
  // 将各种样式的骨架屏组成相应的 
  import {IndexSwiperLoader} from '@/basic/LoaderComponents/index';
  export const IndexPageLoaderType1 = props => (
    <div style={{ background: '#fff' }}>
      <IndexSwiperLoader />
    </div>
  );
  ```

  

##### 加载动画 Loading

- 引用的是 react-lottie

- 先是准备好 动画 json 文件，在将其引入进去即可

  ```javascript
   <Lottie
      width={66}
      height={66}
      options={{
         loop: true, // 不循环
         autoplay: true, // 拉动的时候再开始动
         animationData: loading  // json动画文件
      }}
  />
  ```

- 步骤：（在请求过程中来展示隐藏动画）

  1. 根据接口需不需要展示loading动画

     ```javascript
     export const getIndexModule = data => fetchData('/module/groupModuleDetail/index?version=3', { ...data, showLoading: true }, 'GET'
     ```

   2. 在axios 请求拦截 中判断是否有 showloading 字段

   3.  ```javascript
      if (reqData.showLoading) {
            store.dispatch({
              type: START_LOADING
            })
            delete reqData['showLoading']
          }
      ```

   4. 在axios 响应过程中 隐藏动画

   5. ```javascript
       store.dispatch({
            type: END_LOADING
          });
      ```

##### 登录权限判断

- 
- 

##### 运行环境判断

- 在初始化方法 init 中, 判断浏览器内核

  ```javascript
  const UA = window.navigator.userAgent
  const isIos = UA.includes('ikangiOS/tjb')
  const isAndorid = UA.includes('ikangAndroid/tjb')
  // 主要还是判断 微信浏览器 如果是 则加载微信SDK 调取相应接口 设置 分享参数信息 title desc imgUrl
async function initJSSDk(params) {
    let res = await getWXSignature(window.location.href)
    if (res && res.appResultList && res.appResultList.results) {
      let data = res.appResultList.results[0]
      window.wx.config({
        debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
        appId:
          window.pre_host === 'uat' ? 'wx3c0675db9816ed7c' : 'wxe24e4a001c4553e9', // 必填，公众号的唯一标识（uat和生产环境不同）
        timestamp: data.timestamp, // 必填，生成签名的时间戳
        nonceStr: data.nonceStr, // 必填，生成签名的随机串
        signature: data.signature, // 必填，签名
        jsApiList: [
          'updateAppMessageShareData',
          'updateTimelineShareData',
          'onMenuShareTimeline',
          'onMenuShareAppMessage'
        ] // 必填，需要使用的JS接口列表
      })
      shareContent(params)
    }
  }
  ```
  

##### 如何与原生交互

