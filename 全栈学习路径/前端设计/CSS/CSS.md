# 简介

层叠样式表，主要设计HTML元素的样式。

通常保存在外部的.css文件中。



# 几种添加的方式



对选择器添加样式

选择器可以是：标签，id，类

```css
body { /*对标签，也可以是h1,p等*/
    background-color:#d0e4fe;
}

#para1/*对id = para1的使用以下样式 ，id 不能以数字开头，且id唯一*/
{
    text-align:center;
    color:red;
}

p.center {/*对<p class = "center"></p>的内容居中显示*/
    text-align:center;
}
.center {/*对所有class = "center"的标签生效*/
    text-align:center;
}
.marked p{/*为所有 class="marked" 元素内的 p 元素指定一个样式。*/
}
```

优先级：内联 > 内部 > 外部 > 浏览器默认

选择器的优先级如下，依次递增。

-   通用选择器（*）
-   元素(类型)选择器
-   类选择器
-   属性选择器
-   伪类
-   ID 选择器
-   内联样式

## 外部样式表

样式需要应用到很多页面时，最好用外部样式。只需修改外部文件，就可以修改所有页面的样式。通过放在HTML文件头部的\<link>标签链接。

```css
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
/*
	rel 规定当前文档与被链接文档的样式。stylesheet：文档的外部样式表。
	type 被链接文档的MIME类型。text/css类型描述样式表。
	以上两个值用于外部样式表时基本不变。

	href 文档的链接，可以绝对路径，也可以相对路径。最好用相对路径。文件在迁移时也依然生效。
*/
```

## 内部样式表

单个文档需要特殊样式时，使用内部样式表。

可以理解为单个页面下，样式表放在HTML头部定义，而并非新建一个CSS文件。

```css
<head>
    <style>
    hr {color:sienna;}
    p {margin-left:20px;}
    body {background-image:url("images/back40.gif");}
    </style>
</head>
```



## 内联样式

应用在单个元素上时，用标签内部的style属性。

```css
<p style="color:sienna;margin-left:20px">这是一个段落。</p>
```



-   **Margin(外边距)** - 清除边框外的区域，外边距是透明的。
-   **Border(边框)** - 围绕在内边距和内容外的边框。
-   **Padding(内边距)** - 清除内容周围的区域，内边距是透明的。
-   **Content(内容)** - 盒子的内容，显示文本和图像。

float属性：内容浮动。碰到边框停止。窗口变化时，浮动的内容会自动调整位置。

# 显示

visibility: hidden.不显示且占位

display: none.不显示且不占位

display: inline 。变为内联形式，只占宽度无需换行。

display: block. 变为块形式。

# Position

+   static 默认定位。即没有定位。静态元素，不受top，bottom，left，right影响。
+   fixed 固定定位。即使窗口滚动，元素也不动。相对于浏览器视窗的位置。
+   relative 相对正常定位。调整 top等值，相对正常进行移动。
+   absolute 绝对定位。相对于其最近非static父元素定位，注意可以和其他元素重叠。如果没有父元素，则位置相对于\<html>
+   sticky 粘性定位。介于默认和固定之间。父元素不能是overflow: hidden或者overflow: auto属性。设定top等值的一个阈值。若元素与其父元素的距离大于阈值，就按照默认进行，当距离达到阈值时，就变成固定。

# Overflow

控制内容溢出时的显示方式。

overflow: value。

+   visible：默认，内容出现在元素框之外。
+   hidden：内容被裁剪，外部不可见。
+   scroll：内容被裁剪，但是浏览器会显示滚动条以便查看其余的内容。
+   auto：自动判断是否要用scroll模式。
+   inherit：继承父元素overflow属性。
