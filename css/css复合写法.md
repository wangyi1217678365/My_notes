### css 复合写法

#### Border 属性   
border: 1px solid #000;  
- border-width
- border-style (solid: 实线、dashed: 虚线)
- border-color

#### Margin 和 Padding 属性
margin: 10px 5px 10px 5px;
padding: 10px 5px 10px 5px; 
- 上、右、下、坐 
   
margin: 10px 5px;
padding: 10px 5px;
- 上下、左右

margin: 10px;
padding: 10px;
- 上下左右

#### Background 属性
background: #000 url(images/bg.gif) no-repeat 0/100%;
- background-color (颜色)
- background-image (图片)
- background-report (是否重复背景图片)
- background-position (位置)/background-size (大小) 0 / 100%
- background-origin (定位区域: 图片从容器的什么位置开始显示（border-box | padding-box | content-box，默认值为 padding-box。）)
- background-clipn (绘画区域)
- background-attachment (固定)

#### Font 属性
font: italic bold .8em/1.2 Arial, sans-serif;
- font-style
- font-weight
- font-size
- line-height
- font-family

#### border-radius
border-radius: 10px;
- 上左上右下右下左

border-radius: 10px 20px;
- 上左下左、上右下右

border-radius: 10px 20px 10px 20px;
- 上左、上右、下右、下左
