# SVG 学习笔记

SVG 全称 Scalable Vector Graphics，在 HTML 中使用 HTML 标签进行描述。

## SVG 标签

```html
<svg width="500" height="300" viewbox="0 0 250 150"></svg>
```

* `width` 画布的宽度，支持各种单位和百分比，默认为像素。
* `height` 画布的高度，支持各种单位和百分比，默认为像素。
* `viewbox`属性，由空格或逗号分隔的`4`个值构成，意义分别是`min-x min-y width height`。viewbox 属性定义了画布上可以显示内容的区域，类似于一个截图区域，viewbox 区域的内容会放大到整个画布。
* `preserveAspectRatio` 设置不同的值控制宽高比不匹配时，如何把坐标的范围缩放到视图区域（viewport）

## 形状

```html
<svg width="100%" height="300">
    <rect width="100" height="100" x="100" y="60" rx="6" ry="6" fill="#FF000" /> 
    <circle cx="50%" cy="50%" r="2cm" fill="#009900"></circle>    
    <ellipse cx="25" cy="25" rx="25" ry="15"/>
    <line x1="10" y1="5" x2="30"  y2="50" stroke="#CC0000" />
    <polyline points="0 0, 20 30, 10 60" fill="transparent" stroke="black"/>
    <polygon points="0 0, 20 30, 10 60"/>
</svg>
```

## 矩形 rect

* `x` 左边距，支持相对画布的百分比。
* `y` 上边距，支持百分比。
* `rx` 圆角 x 方向的半径。椭圆的 x 方向的半径。
* `ry` 圆角 y 方向的半径。椭圆的 y 方向的半径。
* `width` 宽度
* `height` 高度

## 圆形 circle

* `cx` 圆心坐标，支持百分比。
* `cy` 圆心坐标，支持百分比。

## 椭圆 ellipse

* `cx` 圆心坐标，支持百分比。
* `cy` 圆心坐标，支持百分比。
* `rx` 椭圆的 x 方向的半径。
* `ry`  椭圆的 y 方向的半径。

## 线段 line

* `x1` 
* `y1`
* `x2`
* `y2`

## 折线 polyline

* `points` 点集数列。每个数字用空白、逗号、终止命令符或者换行符分隔开

## 多边形 polygon

* `points` 点集数列。每个数字用空白、逗号、终止命令符或者换行符分隔开

## 路径 path

每一个命令都有两种表示方式，一种是用大写字母，表示采用绝对定位。另一种是用小写字母，表示采用相对定位

M = moveto 移动到坐标点
L = lineto 线段连接到坐标点
H = horizontal lineto 水平移动多少单位 
V = vertical lineto 垂直移动多少单位 
C = curveto 三次贝塞尔曲线
S = smooth curveto 简写三次贝塞尔曲线
Q = quadratic Bézier curve 二次贝塞尔曲线
T = smooth quadratic Bézier curveto 平滑二次贝塞尔曲线
A = elliptical Arc 弧形
Z = closepath 闭合路径 连接到起点
  
直线/折线

```html
<path d="M10 10 h 80 v 80 h -80 Z" fill="transparent" stroke="black"/>
```

画笔移动到(10,10)点，由此开始，向右移动 80 像素构成一条水平线，然后向下移动 80 像素，然后向左移动 80 像素，最后再回到起点。
  
曲线
  
```html
<path d="M130 110 C 120 140, 180 140, 170 110" stroke="black" fill="transparent"/>
```

C 命令创建三次贝塞尔曲线，需要设置三组坐标参数：C x1 y1, x2 y2, x y (or c dx1 dy1, dx2 dy2, dx dy)，最后一个坐标 (x,y) 表示的是曲线的终点，另外两个坐标是控制点，(x1,y1)是起点的控制点，(x2,y2)是终点的控制点。
  
```html
<path d="M10 80 Q 95 10 180 80" stroke="black" fill="transparent"/>
```

二次贝塞尔曲线Q比三次贝塞尔曲线简单，只需要一个控制点，用来确定起点和终点的曲线斜率。需要两组参数，控制点和终点坐标。Q命令：Q x1 y1, x y (or q dx1 dy1, dx dy)

```html  
<path d="M10 315
             L 110 215
             A 30 50 0 0 1 162.55 162.45
             L 172.55 152.45
             A 30 50 -45 0 1 215.1 109.9
             L 315 10" stroke="black" fill="green" stroke-width="2" fill-opacity="0.5"/>
```

弧形命令A的前两个参数分别是x轴半径和y轴半径，弧形命令A的第三个参数表示弧形的旋转情况，large-arc-flag（角度大小） 和sweep-flag（弧线方向），large-arc-flag决定弧线是大于还是小于180度，0表示小角度弧，1表示大角度弧。sweep-flag表示弧线的方向，0表示从起点到终点沿逆时针画弧，1表示从起点到终点沿顺时针画弧。

## 填充和描边

* `fill` 填充色，如果不想填充任何颜色，设置属性值为`none`。`currectColor`关键字表示继承 CSS 样式中的`color`属性，也用于其他继承用途。
* `fill-opacity` 设置填充的透明度，接受`0`到`1`之间的值。
* `fill-rule` 属性有两个可选值`evenodd`和`nonezero`，当填充纵横交错的多边形时指定重叠区域的填充规则。
* `stroke` 描边颜色，默认值为`none`。
* `stroke-opacity` 描边透明度。
* `stroke-width` 描边宽度。
* `stroke-linecap` 默认值`butt`会紧密修剪描边并与端点垂直，`round`和`square`分别以半圆和方形使用一关的描边宽度来延伸描边。
* `stroke-linejoin` 默认值`miter` 在直线上延伸描边，`round`和`bevel`分别使用圆弧和一根额外的直线连接两条描边。
* `stroke-miterlimit` 延伸斜接线可以超出形状连线的最大距离，是描边宽度的位数（默认是宽度的四倍）。
* `stroke-dasharray` 定义给形状间断描边时的距离模式（线和间隔），如`5,2`。
* `stroke-dashoffset` 定义间断描边时起始偏离的距离，默认值为`0`。

## G 标签

```html
<svg>
    <g id="whiskers">
        <line x1="75" y1="95" x2="135" y2="85" stroke="black" />
        <line x1="75" y1="95" x2="135" y2="105" stroke="black" />
    </g>
    <use xlink:href="#whiskers" transform="scale(-1 1) translate(-140 0)" />
</svg>
```

USE 标签复制了 G 标签的内容。

## 文本

```html
<svg>
    <text x="60" y="165" style="font-family: Consolas; font-weight: bold;">Cat</text>
</svg>
```

## 经验值

* SVG 标签中不能解析 HTML 标签。
* HTML 标签默认在 SVG 层下面，使用`position=relative`或`postion=absolute`可在 SVG 上显示。