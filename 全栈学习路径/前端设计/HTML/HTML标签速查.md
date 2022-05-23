**HTML** **速查列表**

------

HTML 速查列表. 你可以打印它，以备日常使用。

------

**HTML 基本文档**

```html
<!DOCTYPE html> 
<html> 
<head> 
<title>文档标题</title> 
</head> 
<body> 可见文本... </body> 
</html>
```



------

**基本标签（Basic Tags）**

```html
<h1>最大的标题</h1>
<h2> . . . </h2>
<h3> . . . </h3>
<h4> . . . </h4>
<h5> . . . </h5>
<h6>最小的标题</h6>
<p>这是一个段落。</p> 
<br> （换行） 
<hr> （水平线） 
<!-- 这是注释 -->
```



------

**文本格式化（Formatting）**


``` html
<b>粗体文本</b> 
<code>计算机代码</code>
<em>强调文本</em> 
<i>斜体文本</i> 
<kbd>键盘输入</kbd>
<pre>预格式化文本</pre> 
<small>更小的文本</small>
<strong>重要的文本</strong> 
<abbr> （缩写）
<address> （联系信息） 
<bdo> （文字方向）
<blockquote> （从另一个源引用的部分）
<cite> （工作的名称） 
<del> （删除的文本） 
<ins> （插入的文本） 
<sub> （下标文本） 
<sup> （上标文本）
```

------

**链接（Links）**

```html
普通的链接：<a href="http://www.example.com/">链接文本</a> 
图像链接： <a href="http://www.example.com/"><img src="URL" alt="替换文本"></a> 
邮件链接： <a href="mailto:webmaster@example.com">发送e-mail</a> 
书签： <a id="tips">提示部分</a> <a href="#tips">跳到提示部分</a> 
```



------

**图片（Images）**

```html
<img loading="lazy" src="URL" alt="替换文本" height="42" width="42">
```

只设置长或宽，等比放缩。否则强制。单位像素。可设置百分比，按页面比例。

------

**样式/区块（Styles/Sections）**

```html
<style type="text/css">
    h1 {color:red;} p {color:blue;}
</style> 
<div>文档中的块级元素</div>
<span>文档中的内联元素</span>
```

------

**无序列表**

```html
<ul>    
	<li>项目</li>    
	<li>项目</li> 
</ul>
```



------

**有序列表**

```html
<ol>    
	<li>第一项</li>   
	<li>第二项</li> 
</ol>
```



------

**定义列表**

<dl>  
    <dt>项目 1</dt>    
    	<dd>描述项目 1</dd> 
	<dt>项目 2</dt>    
    	<dd>描述项目 2</dd> 
</dl>

------

**表格（Tables）**

<table border="1">   
    <tr>     
        <th>表格标题</th>     
        <th>表格标题</th>   
    </tr>   
    <tr>     
        <td>表格数据</td>    
        <td>表格数据</td>  
    </tr> 
</table>

------

**框架（Iframe）**

```html
<iframe src="demo_iframe.htm"></iframe>
```



------

**表单（Forms）**

```html
<form action="demo_form.php" method="post/get"> 
    <input type="text" name="email" size="40" maxlength="50"> 
    <input type="password"> 
    <input type="checkbox" checked="checked"> 	/
    <input type="radio" checked="checked">      //单选
    <input type="submit" value="Send"> 
    <input type="reset"> 
    <input type="hidden"> 
    <select> <option>苹果</option> <option selected="selected">香蕉</option> <option>樱桃</option> </select> 
    <textarea name="comment" rows="60" cols="20"></textarea>  
</form>

使用<label id = " "> </label>和input标签绑定，只需点击label的文字，就会自动聚焦到输入框。
```



------

**实体（Entities）**

```
&lt; 等同于 <
&gt; 等同于 >
&#169; 等同于 ©
```



# 一些缩写

ul是unordered lists的缩写 (无序列表)

li是list item的缩写 （列表项目）

ol是ordered lists的缩写（有序列表）

dl是definition lists的英文缩写 (自定义列表)

dt是definition term的缩写 (自定义列表组)

dd是definition description的缩写（自定义列表描述）

nl是navigation lists的英文缩写 （导航列表）

tr是table row的缩写 （表格中的一行）

th是table header cell的缩写 （表格中的表头）

td是table data cell的缩写 （表格中的一个单元格）
