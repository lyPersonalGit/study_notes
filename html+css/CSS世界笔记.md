## 一、块级元素宽高特性

 

### 1.width



1. 当不为块级元素设置width属性时，width属性的默认值是auto，块级元素则具有流动性，即自动填满最大宽度，充分利用可利用空间。
2. 当为块级元素的width属性设置具体值时，则该元素失去流动性，并且其宽度属性作用与元素的content-box上，元素实际表现出来的宽度为，content-box的宽度加上左右padding值加上左右border的值
3. 当为块级元素设置宽度后，仍使它保持流动性的方法：使用宽度分离原则（即使用父子元素，父元素设置宽度，子元素不设置宽度），或者给元素设置box-sizing：border-box；

### 2.height



1. 所有元素的高度默认为auto，高度的计算值相对于父元素，若想使元素height：100%生效，需要父元素有确定的高度值，需要让元素高度全屏时，则需要为html，body元素设置height：100%。

### 3.max-width与min-width,max-height与min-height



max-width属性与min-width属性的设置不会改变具有流动性元素的流动性特性。

### 4.格式化宽高



当position属性为absolute时，当left与right的属性值同时存在时，元素的宽度表现为格式化宽度，当top与bottom属性值同时存在时，元素的高度表现为格式化高度，格式化宽高具有完全的流动性



## 二、盒模型



### 1.content

1. 非替换元素设置content：url()属性时，可将该元素显示为图片，但该图片的宽高取决于该图片资源的宽高，不能被width与height属性修改
2. 图片元素设置content：url()属性，可以修改该图片的图片资源，图片宽高仍受width与height属性的控制。
3. :before伪元素和:after伪元素的生成需要设置content属性，可以设置的content属性有content:""，content:attr()
4. :before伪元素和:after伪元素可以设置content:url()属性，但是不推荐伪元素用content:url()来生成图片，推荐设置content:''属性再添加background-image来生成图片



### 2.padding

1. 内联元素的垂直方向的padding属性，可以影响视觉表现，对上下元素原本布局无影响。（可以凭借此特性，来给a标签增加实际点击范围）
2. padding属性支持百分比值，padding的百分比值无论水平方向还是垂直方向都是相对于宽度来计算的。



### 3.margin

1. margin属性无法对已设定宽度的元素产生宽度影响，而具有流动性的元素设置margin属性来改变尺寸。

2. margin属性中的auto值只能对有宽度的块级元素生效，其含义为margin自动填充剩余的空间（可借此来实现有宽度的块级元素的左中右对齐，即在要对齐的相对方向设置margin值为auto）

3. 只有块级元素（float与absolute除外）才会出现margin合并，margin合并出现的情况：1.子级盒子的margin合并到了父级上；2.相邻盒子的margin合并在一起（取较大的值）；

4. 子元素的margin-top合并到父元素上的解决方法：父元素设置bfc、父元素设置border-top、父元素设置padding-top，子元素的margin-bottom合并到父元素上的解决方法：父元素设置bfc、父元素设置border-bottom、父元素设置padding-bottom



## 三、内联元素的表现



### 1.内联盒模型

1. 内容区域，html中文字所占用的盒子。
2. 内联盒子，表现为display：inline的元素（不包括内联元素）
3. 当内联盒子存在时，内联盒子的那一行则视为一个行框盒子。而每个行框盒子的前面会出现一个幽灵空白节点。display：inline-block的元素和其他大多数替换元素也表现为行框盒子的特性。



### 2.line-height

1. 非替换元素的纯内联元素，其可视高度完全由line-height决定，对于块级元素，line-height对其本身没有任何作用，实际上块级元素的高度发生变化，是通过改变块级元素中的内联级别元素占据的高度实现的。
2. line-height支持数值，百分比，长度值，这三种类型的值在后代继承后的表现不同，其中后代继承百分比值与长度值时，继承的是父元素通过百分比与长度值计算出来的值，而后代继承数值时，则单纯继承比值，再作用到当前元素计算。
3. 行框盒子的高度是由高度最高的那个内联盒子决定的。



### 3.vertical-align

1. 内联元素的vertical-align属性默认为baseline，即默认以x字母的下边缘对齐，而vertical-align：top属性，则是让该内联元素与当前行框盒子的顶部对齐，vertical-align：bottom属性，是让该内联元素与当前行框盒子的底部对齐。
2. vertical-align：baseline是基于字体的下边缘对齐的，那么如果有两个行高相同，字体大小不同的内联盒子，那么字体大的那个内联盒子的baseline基准线则会更向下偏移。



### 4.幽灵空白节点

幽灵空白节点是存在于每个行框盒子前面，具有字体与行高属性，宽度为零的内联盒子。

1. 幽灵空白节点引发父级盒子高度改变或者同级元素错位的原因有：

- 内联盒子vertical-align默认值是baseline（也就是字体的下边缘）

 ``` html
<div class="box">
    <img src="1.png" >
</div>
 ```

- baseline基准线不同（父级盒子与子级内联盒子的字体大小不同）

 ``` html
<div class="box" style="line-height: 32px;">
    <span style="font-size:24px;">文字</span>
</div>
 ```

2. 知道了幽灵空白节点的特性，那么解决幽灵空白节点影响的方法就有：

- 把内联盒子改为块级元素（没有行框盒子，幽灵空白节点自然不存在）
- 将父级元素的font-size设置为0（调整字体就是调整baseline的位置）
- 将父级元素的line-height设置为0（使幽灵空白节点不足以撑起盒子）
- 将内联元素的水平对齐方式改为top，bottom或middle



## 四、脱离文档流与保护文档流



### 1.BFC

1. 有块状格式化上下文的元素的表现为，该元素外的元素不影响该元素内的元素，该元素内的元素也不影响该元素外的元素。块状格式化的作用：

- 例如块状格式化的元素内部的元素设置的margin不会合并到外部父级元素。
- 例如块状格式化元素内的元素不会受到外部浮动元素的浮动影响。

2. 拥有块状格式化上下文特性的元素有：

- float，元素设置float属性后变为脱离文档流的块级元素，具有包裹性，使左右元素浮动的特性
- absolute，元素设置position属性后变为脱离文档流的块级元素，绝对定位元素具有包裹性，且脱离文档流
- inline-block，display为inline-block元素，具有包裹性，属于行内元素
- overflow，设置overflow为hidden的元素，不具有包裹性，但设置在块级元素上时，可以保持块级元素的流动性。



### 2.float

1. 浮动元素脱离当前文档流，这意味着如果具有浮动属性的元素的父元素内没有其他的元素及内容，且父元素没有指定数值高度，则父元素的高度会塌陷（高度表现为0）。

 ``` html
<p>
    <img style="float: left; width: 40px; height: 40px;" />
</p>
 ```



2. 浮动元素的设计本意是设置在图片上实现文字环绕的效果，行框盒子与浮动元素不可重叠。这就表明浮动元素上下文的其他行框盒子，只能处于其后，或者在向后一行排列

 ``` html
<p style="width: 100px;">
    <img style="float: left; width: 40px; height: 40px;" />
  	中文中文中文中文中文中文中文中文
</p>
 ```



3. 浮动元素脱离当前的文档流，但是在当前行框盒子占据一个位置，所占据的位置，实际上是一个行框盒子所占据的位置（实际display为block），float元素的定位不是依据自己的父级元素来定位，实际上是依据当前所在的行框盒子向左或向右定位。所以浮动元素具体的位置所在，要看设置浮动属性前所在行框盒子的位置。

``` html
<p style="width: 100px;">
  	中文中文中文中文中文中文中文中文
    <a style="float: right;" href="">更多</a>
</p>
```

 ``` html
<p style="width: 100px;">
  	中文中文中文中文中文中文中文中文
    <a style="float: left;" href="">更多</a>
</p>
 ```



### 3.overflow

1. overflow属性默认值为visible，auto值为超出盒子范围后才出现滚动条，scroll值为滚动条一直出现，hidden值为超出范围后隐藏。overflow：hidden的裁剪边界为padding-box
2. overflow-x或overflow-y属性，其中一个设置值，则另个值默认表现为auto，除非两个属性同时设置值。



### 4.position:absolute

1. position：absolute的包含块为position不为static的最近的祖先元素，定位边界为包含块的padding-box；
2. left，right，top，bottom定位属性，当对立方向只有一侧有值时，position：absolute的盒子表现为包裹性（盒子大小由内容决定，且不超过包含块大小），当对立方向的两侧都有值时，则盒子表现为格式化宽高，具有流动性。（这些特性建立在盒子未设置宽高的基础上）
3. 无依赖absolute绝对定位。当left，right，top，bottom定位属性都不设置值，且祖先元素不设置relative值时，absolute元素表现为无依赖绝对定位，可以理解为脱离文档流，不占文档流位置的相对定位。
4. 通过absolute属性实现水平垂直居中，首先需要给元素设置宽高，再给定位属性的对立方向的两侧都设值，再设置margin：auto即可。



### 5.position:relative

1. relative定位相对于自身定位，不破坏文档流。当设置left，right，top，bottom定位属性时，对立方向只有一侧有效。



### 6.position:fixed



## 五、层叠上下文关系



### 1.层叠顺序规则

background/border < 负z-index < block块状水平盒子 < float浮动盒子 < inline水平盒子 < z-index:auto/z-index:0 < 正z-index，可以理解为内容大于布局大于装饰