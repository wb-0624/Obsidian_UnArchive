可插入HTML页面的脚本语言编程代码。



HTML中的脚本必须放在\<script>\</script>之间。

脚本可以被放置在\<body>\<head>中。

# 脚本添加方式

## 内部脚本

尽量统一放在一起，方便阅读。并且放在最后。

```css
<html>
	<head>
		<script>
			............
		</script>
	</head>
	<body>
		<script>
			............
		</script>
	</body>
</html>
```



## 外部脚本

```css
<script src = "URL/XXX.js">
</script>
```

外部脚本编写时，无需标签，直接写js代码即可。



# 输出

+   window.alert()弹出警告框。
+   document.write()将内容写入HTML文档中。
+   innerHTML写入到HTML元素。
+   console.log()写入到浏览器的控制台。