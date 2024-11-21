---
title: 用纯 CSS 实现网格背景
date: 2024-11-21 17:57:48
categories: 技术
tags: CSS
---

是不是在日常开发中经常遇到实现网格的需求，网格通常对网页中展示的元素能起到很好的定位和对齐作用。

这里介绍如何只通过 CSS 来实现这个需求？

<!-- more -->

## 使用背景图

这里我们的背景图使用 SVG 来创建，首先，创建绘出一个正方形，填充白色；然后通过矩形实现垂直和水平的线条，进而分别对它们进行定位居中。

```html
<svg xmlns='http://www.w3.org/2000/svg' width='40' height='40'>
  <rect width='40' height='40' fill='#fff'></rect>
  <rect x='50%' width='1' height='100%' fill='rgb(203 213 225)'></rect>
  <rect y='50%' width='100%' height='1' fill='rgb(203 213 225)'></rect>
</svg>
```

效果如下：

<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40"><rect width="40" height="40" fill="#fff"></rect><rect x="50%" width="1" height="100%" fill="rgb(203 213 225)"></rect><rect y="50%" width="100%" height="1" fill="rgb(203 213 225)"></rect></svg>

有了背景图片，我们对给定的区域设置背景：

```css
.grid {
  background-image: url('/path/to/grid.svg');
}
```

如果要避免加载额外的资源，我们也可以通过图片二进制数据的方式嵌入：

```css
.grid {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='40' height='40'%3E%3Crect width='40' height='40' fill='%23fff' /%3E%3Crect x='50%' width='1' height='100%' fill='rgb(203 213 225)' /%3E%3Crect y='50%' width='100%' height='1' fill='rgb(203 213 225)' /%3E%3C/svg%3E%0A");
}
```

默认情况下，背景图像会在垂直和水平方向上重复，这样实现的网格是40个像素。我们也可以通过 background-size 属性来自定义背景图的尺寸。

<div style="width:100%;height:200px;margin-bottom:20px;background-image:url(&quot;data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='40' height='40'%3E%3Crect width='40' height='40' fill='%23fff' /%3E%3Crect x='50%' width='1' height='100%' fill='rgb(203 213 225)' /%3E%3Crect y='50%' width='100%' height='1' fill='rgb(203 213 225)' /%3E%3C/svg%3E%0A&quot;);background-size:40px"></div>

```css
.grid {
  background-size: 20px;
}
```
<div style="width:100%;height:200px;margin-bottom:20px;background-image:url(&quot;data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='40' height='40'%3E%3Crect width='40' height='40' fill='%23fff' /%3E%3Crect x='50%' width='1' height='100%' fill='rgb(203 213 225)' /%3E%3Crect y='50%' width='100%' height='1' fill='rgb(203 213 225)' /%3E%3C/svg%3E%0A&quot;);background-size:20px"></div>

这里有个缺点，就是调节图片显示大小的时候，线条的宽度也会有变化。就会导致上面的40像素的网格和20像素的网格线条粗细不一致。

或许最好的做法是改变 SVG 文件里的参数 width 和 height 来调整尺寸：

```css
.grid {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='20' height='20'%3E%3Crect width='20' height='20' fill='%23fff' /%3E%3Crect x='50%' width='1' height='100%' fill='rgb(203 213 225)' /%3E%3Crect y='50%' width='100%' height='1' fill='rgb(203 213 225)' /%3E%3C/svg%3E%0A");
}
```
<div style="width:100%;height:200px;margin-bottom:20px;background-image:url(&quot;data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='20' height='20'%3E%3Crect width='20' height='20' fill='%23fff' /%3E%3Crect x='50%' width='1' height='100%' fill='rgb(203 213 225)' /%3E%3Crect y='50%' width='100%' height='1' fill='rgb(203 213 225)' /%3E%3C/svg%3E%0A&quot;);background-size:20px"></div>

## 使用线性渐变

另一种使用 CSS 创建网格背景的方式是通过 linear-gradient() 函数来实现的。

首先选择要为其添加网格背景的元素，并设置 background-image 属性。然后，使用 linear-gradient() 函数 指定两种颜色，这两种颜色可以是相似或相同的，它们之间由线条粗细、宽度相同的透明部分分隔开来。

```css  
.grid {
  background-image: linear-gradient(to right, gray 1px, transparent 1px);
}
```

<div style="width:100%;height:200px;margin:20px 0;background-size:20px 20px;background-position:center center;background-image:linear-gradient(to right, rgb(203 213 225) 1px, transparent 1px)"></div>

这里的 1px 代表网格的线条宽度，你也可以设置成你想要的宽度。

如果要实现水平线条，只需要把 to right 改为 to botton 。而网格实现既需要横向的线条，也需要纵向的线条。

```css
.grid {
  background-image:
    linear-gradient(to right, gray 1px, transparent 1px),
    linear-gradient(to bottom, gray 1px, transparent 1px);
}
```
<div style="width:100%;height:200px;margin:20px 0;background-size:20px 20px;background-position:center center;background-image:linear-gradient(to right, rgb(203 213 225) 1px, transparent 1px),linear-gradient(to bottom, rgb(203 213 225) 1px, transparent 1px)"></div>


完整的的样式代码如下：

```css
.grid {
  height: 200px;
  width: 100%;
  background-image:
      linear-gradient(to right, rgb(203 213 225) 1px, transparent 1px),
      linear-gradient(to bottom, rgb(203 213 225) 1px, transparent 1px);
  background-size: 20px 20px;
  background-position: center center;
}
```

## 边缘虚化

还有一种场景需要将网格的边缘进行虚化，这时需要借助 mask-image 属性设置用作元素蒙版层的图像，它的值参见 文档，这里通过径向渐变的方式实现：

```css
.grid {
  -webkit-mask-image: radial-gradient(ellipse 50% 50% at 50% 50%,#000 70%,transparent 100%);
  mask-image: radial-gradient(ellipse 50% 50% at 50% 50%,#000 70%,transparent 100%);
}
```
<div style="width:100%;height:200px;margin:20px 0;background-size:20px 20px;background-position:center center;background-image:linear-gradient(to right, rgb(203 213 225) 1px, transparent 1px),linear-gradient(to bottom, rgb(203 213 225) 1px, transparent 1px);webkit-mask-image:radial-gradient(ellipse 50% 50% at 50% 50%,#000 70%,transparent 100%);mask-image:radial-gradient(ellipse 50% 50% at 50% 50%,#000 70%,transparent 100%)"></div>

如果你想要四周线性渐变的遮罩效果，则使用如下的代码：

```css
.grid {
  -webkit-mask-image: linear-gradient(to bottom,transparent,#fff 50px calc(100% - 50px),transparent),linear-gradient(to right,transparent,#fff 50px calc(100% - 50px),transparent);
  mask-image: linear-gradient(to bottom,transparent,#fff 50px calc(100% - 50px),transparent),linear-gradient(to right,transparent,#fff 50px calc(100% - 50px),transparent);
  mask-composite: intersect;
  -webkit-mask-composite: source-in, xor;
}
```
<div style="width:100%;height:200px;margin:20px 0;background-size:20px 20px;background-position:center center;background-image:linear-gradient(to right, rgb(203 213 225) 1px, transparent 1px),linear-gradient(to bottom, rgb(203 213 225) 1px, transparent 1px);webkit-mask-image:linear-gradient(to bottom,transparent,#fff 50px calc(100% - 50px),transparent),linear-gradient(to right,transparent,#fff 50px calc(100% - 50px),transparent);mask-image:linear-gradient(to bottom,transparent,#fff 50px calc(100% - 50px),transparent),linear-gradient(to right,transparent,#fff 50px calc(100% - 50px),transparent);mask-composite:intersect;webkit-mask-composite:source-in, xor"></div>

## 网状点阵背景

基于相同的原理，我们来实现一个网状点阵背景，这里需要用到 radial-gradient 函数，创建圆形填充背景色。

```css
.grid {
  height: 200px;
  background-image: radial-gradient(circle, rgb(203 213 225) 2px, #fff 2px);
  background-size: 20px 20px;
  background-position: center center;
}
```
<div style="height:200px;background-image:radial-gradient(circle, rgb(203 213 225) 2px, #fff 2px);background-size:20px 20px;background-postion:center center"></div>

## 拓展知识

另外关注一下这种模拟透明背景样式的实现原理，它同样可以使用在构建格子花纹的图案上。

```css
.grid {
  height: 200px;
  width: 100%;
  background-image:
    linear-gradient(45deg, #8d8b8b 25%, transparent 0),
    linear-gradient(-45deg, #8d8b8b 25%, transparent 0),
    linear-gradient(45deg, transparent 75%, #8d8b8b 0),
    linear-gradient(-45deg, transparent 75%, #8d8b8b 0);
  background-position: 0 0, 0 10px, 10px -10px, -10px 0;
  background-size: 20px 20px;
}
```
<div style="height:200px;background-image:linear-gradient(45deg,#8d8b8b 25%,transparent 0),linear-gradient(-45deg,#8d8b8b 25%,transparent 0),linear-gradient(45deg,transparent 75%,#8d8b8b 0),linear-gradient(-45deg,transparent 75%,#8d8b8b 0);background-size:20px 20px;background-position:0 0, 0 10px, 10px -10px, -10px 0;inset:0"></div>
<!-- 
## 生成工具
 -->
<!-- 这里为了根据需要快速获得代码，做了一款生成工具： -->

<!-- https://spacexcode.com/blog/pure-css-grid-line/ -->