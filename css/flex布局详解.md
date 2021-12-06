<style>
    @import url('../index.css')
</style>
### 基本概念
使用 flex 布局首先要设置父容器 display: flex，然后再设置 justify-content: center 实现水平居中，最后设置 align-items: center 实现垂直居中。
```
#dad {
    display: flex;
    justify-content: center;
    align-items: center
}
```  
![flex基本概念](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/221bb6de73e54f4104a1.png~tplv-t2oaga2asx-watermark.awebp)  
<p class="bg">
  flex 的核心的概念就是 容器 和 轴。容器包括外层的 父容器 和内层的 子容器，轴包括 主轴 和 交叉轴，可以说 flex 布局的全部特性都构建在这两个概念上。flex 布局涉及到 12 个 CSS 属性（不含 display: flex），其中父容器、子容器各 6 个。不过常用的属性只有 4 个，父容器、子容器各 2 个，我们就先从常用的说起吧。
</p>     
            
### 1. 容器 
> 容器具有这样的特点：父容器可以统一设置子容器的排列方式，子容器也可以单独设置自身的排列方式，如果两者同时设置，以子容器的设置为准。  
![容器](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/f443b657dbc39d361f68.png~tplv-t2oaga2asx-watermark.awebp)      

#### 1.1 父容器
- 设置子容器沿主轴排列：justify-content
 justify-content 属性用于定义如何沿着主轴方向排列子容器。  
 ![justify-content属性](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/be5b7f0e022a8da60ed8.png~tplv-t2oaga2asx-watermark.awebp)     
  - flex-start：起始端对齐
  - flex-end：末尾段对齐
  - center：居中对齐
  - space-around：子容器沿主轴均匀分布，位于首尾两端的子容器到父容器的距离是子容器间距的一半。
  - space-between：子容器沿主轴均匀分布，位于首尾两端的子容器与父容器相切。
 
- 设置子容器沿交叉轴排列：align-items
 align-items 属性用于定义如何沿着交叉轴方向分配子容器的间距。  
 ![align-items属性](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/e7e6aa079f5333828c58.png~tplv-t2oaga2asx-watermark.awebp)      
  - flex-start：起始端对齐
  - flex-end：末尾段对齐
  - center：居中对齐
  - baseline：基线对齐，这里的 baseline 默认是指首行文字，即 first baseline，所有子容器向基线对齐，交叉轴起点到元素基线距离最大的子容器将会与交叉轴起始端相切以确定基线。  
  - ![交叉轴基线对齐](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/f78e9f42be9a3f165f8f.png~tplv-t2oaga2asx-watermark.awebp)  
  - stretch：子容器沿交叉轴方向的尺寸拉伸至与父容器一致。  
  - ![子容器沿交叉轴方向拉伸至与父容器一致](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/160170b3d2022800ffea.png~tplv-t2oaga2asx-watermark.awebp)

#### 1.2 子容器
- 在主轴上如何伸缩：flex  
![flex属性](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/78e9030183f686e0b6ed.png~tplv-t2oaga2asx-watermark.awebp)  
`子容器是有弹性的（flex 即弹性），它们会自动填充剩余空间，子容器的伸缩比例由 flex 属性确定。flex 的值可以是无单位数字（如：1, 2, 3），也可以是有单位数字（如：15px，30px，60px），还可以是 none 关键字。子容器会按照 flex 定义的尺寸比例自动伸缩，如果取值为 none 则不伸缩。
`
  - flex-grow: 规定子容器将相对于其他灵活的项目进行扩展的量
  - fiex-shrink：一个数字，规定项目将相对于其他灵活的项目进行收缩的量。
  - flex-basis：项目的长度。合法值："auto"、"inherit" 或一个后跟 "%"、"px"、"em" 或任何其他长度单位的数字。
  - flex：1 === flex-grow：1 flex-shrink：1 flex-basis：0
  
- 单独设置子容器如何沿交叉轴排列：align-self    
![align-self属性](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/1d09fe5bb413a6dfa5dd.png~tplv-t2oaga2asx-watermark.awebp)    
每个子容器也可以单独定义沿交叉轴排列的方式，此属性的可选值与父容器 align-items 属性完全一致，如果两者同时设置则以子容器的 align-self 属性为准。

### 2. 轴
> 如图所示，轴 包括 主轴 和 交叉轴，我们知道 justify-content 属性决定子容器沿主轴的排列方式，align-items 属性决定子容器沿着交叉轴的排列方式。那么轴本身又是怎样确定的呢？在 flex 布局中，flex-direction 属性决定主轴的方向，交叉轴的方向由主轴确定。    
![轴](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/5f2a17efffe8f3ab78a4.png~tplv-t2oaga2asx-watermark.awebp)    
#### 主轴（flex-direction）
主轴的起始端由 flex-start 表示，末尾段由 flex-end 表示。不同的主轴方向对应的起始端、末尾段的位置也不相同。
- row：主轴从左向右
- column： 主轴从上向下
- row-reverse：主轴从右向左
- column-reverse：主轴从下向上
<p class="danger">交叉轴的方向跟主轴的方向是相互独立互不影响的它们相互垂直</p>


### flex 进阶概念
#### 1. 父容器
- 设置换行方式：flex-wrap（决定子容器是否换行排列，不但可以顺序换行而且支持逆序换行。）
- ![flex-wrap](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/19fb0f3a31fa497191b8.png~tplv-t2oaga2asx-watermark.awebp)
  - wrap-reverse：逆序换行（逆序换行是指沿着交叉轴的反方向换行。）
- 轴向与换行组合设置：flex-flow（flow 即流向，也就是子容器沿着哪个方向流动，流动到终点是否允许换行，比如 flex-flow: row wrap，flex-flow 是一个复合属性，相当于 flex-direction 与 flex-wrap 的组合，可选的取值如下：）     
  - row、column 等，可单独设置主轴方向
  - wrap、nowrap 等，可单独设置换行方式
  - row nowrap、column wrap 等，也可两者同时设置
- 多行沿交叉轴对齐：align-content（当子容器多行排列时，设置行与行之间的对齐方式。）
- ![align-content](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/ff9bd219375f048b3304.png~tplv-t2oaga2asx-watermark.awebp)
  - flex-start：起始端对齐
  - flex-end：末尾段对齐
  - center：居中对齐
  - space-around：等边距均匀分布
  - space-between：等间距均匀分布
  - stretch：拉伸对齐
  - ![拉伸对齐](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/878b39463db6bc499fbc.png~tplv-t2oaga2asx-watermark.awebp)
### 2. 子容器
  - 设置基准大小：flex-basis （表示在不伸缩的情况下子容器的原始尺寸。主轴为横向时代表宽度，主轴为纵向时代表高度。）
  - 设置扩展比例：flex-grow （子容器弹性伸展的比例。如图，剩余空间按 1:2 的比例分配给子容器。）
  ![flex-grow](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/bcca55b82d18e2ac2367.png~tplv-t2oaga2asx-watermark.awebp)
  ![flex-grow](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/72e9f508dff25a474b40.png~tplv-t2oaga2asx-watermark.awebp)
  - 设置收缩比例：flex-shrink （子容器弹性收缩的比例。如图，超出的部分按 1:2 的比例从给子容器中减去。）
  ![flex-shrink](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/38596937d4f86beeac0b.png~tplv-t2oaga2asx-watermark.awebp)
  ![flex-shrink](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/d278e36c13b9643ff481.png~tplv-t2oaga2asx-watermark.awebp)
  - 设置排列顺序：order。改变子容器沿主轴的排列顺序，覆盖 HTML 代码中的顺序，默认值为 0，可以为负值，数值越小排列越靠前。
  ![order](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/4eb20f9bfc611e66b069.png~tplv-t2oaga2asx-watermark.awebp)

  总：
  ![总图](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/leancloud-assets/0dd26d8e99257ff36443.png~tplv-t2oaga2asx-watermark.awebp)