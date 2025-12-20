+++
date = '2025-12-01T23:41:08+08:00'
draft = false
title = 'Html和css文件设计语法'
weight = 151
description = 'html文件是浏览器能读懂的语法，文件可以在浏览器打开 | css是html文件的配置文件，配置html文件中元素的样式'
+++


# html概览
```
<!-- 头标前 -->
<head>
    <style>
    <!-- css文件嵌入html -->
    </style>
</head>
<!-- 主体标签 -->
<body>
</body>
```  

* 元素的逻辑：兄弟关系 + 父子关系
* 文档流原则：宽占一行，高度由内容撑开
* link标签 ，实现css文件嵌入html：
`<link rel="stylesheet" href=".\相对路径-->指向css文件">替代原本的<style></style>部分 | 标签内部<style="标签内置">`

### html的标签——元素
##### 标签

* 链接标签* `<a href="链接">文本</a>`  
* 图片标签* `<img src="链接" alt="图片加载失败">`    
    1. 长度比例恒定
* 输入标签*  
`<input>单行输入` 
`<textarea></textarea>多行输入`   



***
# css概览 

##### 属性
* ***对于某个元素个体的设置：***  
> opcity : (0 -> 1<sup>[渐实体]</sup>),元素整体<sup>包括背景和内容</sup>透明度设置   
> background : url(路径) center/cover no-repeat,设置背景
> float ：(left|right),打破文档流----吸引元素  
> overflow : ( auto | hidden ),超出元素范围的内容处理  
> transform : ( translate( a<sup>右</sup>,b<sup>下</sup> ) | rotate(X|Y|Z)( a<sup>单位：deg</sup> ) | scale( a<sup>x轴</sup>,b<sup>y轴</sup> ) )

> 元素显示： 
    1. display : ( block | inline<sup>行内</sup> | inline-block<sup>兼具</sup> )  
    2. inline元素的margin属性只能( left|right )水平设置 ； padding属性只能( top|bottom )纵向设置

> 盒子模型：  
    1. padding | border | margin <sup>一种空间配置属性，参照父类空间的边界或其他子元素</sup>[*](#margin空间配置属性)  
    2. box-sizing:border -box,盒子模型用面积内缩实现
    3.boder-radius 圆角设计<sup>把一个方形元素设置border-radius : 50% ---> 圆形元素</sup>

> 元素空间定位：  
    1. text-align : ( left | center | right ),为元素内容定位    
    2. position : ( relative | absoluate | fixed ) ，都是为元素整体[定位](#元素定位)  
    3. z-index ：元素显示的图层，*只有含有`position`定位的元素才能设置

> 弹性布局：  
    1. display : flex,父元素设置

    2. 配置 -主轴-方向：flex-direction : ( row<sup>---></sup> | row-reverse )  
    配置 -主轴-子元素位置： justify-content : ( left | center | right )  ;  justify-content : ( space-between | space-around | space-evently )

    3. 配置 -交叉轴-属性同上 ：align-items
    4. flex -wrap : wrap,子元素超出父元素宽度时，不会再变形填充，而是自动换行。
    5. 配置 -多行-交叉轴-属性同上 ： align-content

    6. flex : 1<sup>权数</sup>,子元素配置，按权数比例填充父类

> 动画：
    1. animation:[ animation-name<sup>用于关联[@keyframes](#动画对应的关键帧)</sup> | animation-duriation<sup>总时长</sup> | animation-dalay<sup>延迟开始</sup> | animation-iteration-count<sup>播放次数</sup> | ] 


***
##### 非常规设计css
* 鼠标悬停：  
> 1. 选择器：hover{悬停该元素时效果，*由于是动画性质，可以加入`transition:`配置变化效果}  
> 2. 选择器：hover 选择器{悬停该元素时，影响他的子元素属性}  
> 3. 选择器：hover ~ 选择器{悬停该元素时，影响他的兄弟元素属性}

* 伪元素：
> 可以实现“清楚元素浮动”  
> 选择器<sup>含float设置的浮动元素</sup> ：after{  
    content : ""  
    clear : both  
    display : block  
}  
> *在这个元素后面的元素不再受该元素float的吸引*

* #### 动画对应的关键帧：
> @keyframes 动画名称{  
    0%{配置css}  
    a%<sup>可以加入任意的帧</sup>{配置css}  
    100%{配置css}  
}


***
##### 选择器  
* id选择器：
> <id="命名">  
> 对应的css用 #命名{ }  

* class选择器：
> <class="命名">  
> 对应的css用 .命名{ }

* <标签>选择器：  
> 对应的css用 标签名{ }

* 父元素 子元素{ }
> 先锁定父元素，再找其中的子元素进行配置

* 父元素 ： nth-child (次序){ }
> 先锁定父元素，再找其中对应次序的子元素进行配置


***
#### 现象

##### margin，空间配置属性
*margin纵向空间塌陷:
>  margin-(top|botttom)纵向属性设置，且参照 父元素空间边界  
> 由于 父类元素没有盒子模型属性设置  
> 子元素 穿透参照祖先元素
> *处理方法* ：**父元素设置overflow ：hidden，忽视祖先元素**

##### !important 权重设置  
    `在 属性：数值 !important 设置--> 覆盖其他地方的该属性的数值`
  
##### 元素定位  
relative : 不脱离文档流，参照元素原本位置的左上角  
absoluate : 脱离，参照祖先元素<sup>第一个有position定位的</sup> ，我们可以父元素设置设置position ： relative,无影响定位父元素  
fixed : 脱离，参照整个页面  

*  定位类型确定后，用( top | bottom | left | right )，设置具体位置
