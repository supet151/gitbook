# HTML基础部分
# 页面结构
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>标题</title>
</head>
<body>
<p>内容</p>
</body>
</html>
```
组成页面的必备标签：
`<!DOCTYPE html>` 声明为 HTML5 文档
`<html>`是HTML页面的根元素
`<head>`元素包含了文档的元（meta）数据
`<title>`元素描述了文档的标题
- 声明为utf-8编码`<meta charset="utf-8">`，在大多数情况下，直接输入中文会出现乱码
- 声明为gbk编码`<meta charset="gbk">` 


`<body>`描述了页面的内容,仅有body中的呢日哦给你才会在浏览器中显示

标签分为：开始标签和结束标签
`<标签>内容</标签>`

# HTML属性
html元素：
- 以开始标签开始
- 以结束标签终止
- 在开始标签与结束标签之间书写**元素内容**
- 有些html元素是没有元素内容的，空元素应该在开始标签中进行关闭，即元素是自闭合标签
- html的属性应该写在开始标签，且属性值应该始终被包括在引号内，如果属性值本身含有双引号，应该使用单引号包括属性值。
有一些常见的属性：
- class：为html元素定义一个或者多个类名，以便样式文件引用
- id：定义元素**唯一**的id
- style:规定元素的行内样式(inline style)
- title:描述了元素的额外信息

html页面会忽略页面中的排版，会把多个空格认为一个空格，但是不会把多个`<br>`认为是一个。

定义注释：`<!-- xxx -->`
# 基础标签
基础标签都是写在`<body>`中
w3c推荐使用小写标签
## html标题
通过`<h1>-<h6>` 标签定义标题大小，数字越小标题越大,这个标题标签不是指页面标题,1-6号标题与1-6号字体逆序对应

应该将html标题只用于标题，不应该为了生成粗体文本与大号文本而使用标题；
搜索引擎使用标题为网页的结构和内容编制索引
应该以h1作为主标题
### 水平线
`<hr>`标签在html页面中创建水平线，可以用来分隔内容
## html段落
html段落通过`<p>`标签定义
## html链接
html链接通过`<a>`定义
```
<a href="http://xxx">这是链接</a>
```
在href属性中指定链接的地址，可以通过css修改默认的链接样式
target属性定义了被显示的文档在何处显示，`target="_blank"`时，会在新窗口打开文档
id属性可用于创建在一个HTML文档书签标记。
可以使用：
```
//id="tips"
<a href="#tips"></a>来指向书签
<a href="xxx.html#tips"></a>这样的指向也可
```
<strong>
	应该始终将正斜杠添加到子文件夹，如果不添加，会产生两次的http请求，因为服务器会添加正斜杠到这个地址，然后创建一个新的请求。
</strong>

## html图像
```
<img src="..." alt="" width="" height=""/>
```
图像是一个自闭合标签，src属性指定了图像的地址链接，alt指定了在图片无法正常加载时显示的文字,另外两个属性指定了图像的大小
`<map>`定义了图像映射，图像映射指带有可点击区域的一幅图像，将`<area>`作为其子DOM可以自定义点击图片不同位置时的链接。
HTML5 中, 如果 id 属性在<map> 标签中指定, 则你必须同样指定 name 属性。
## html空元素
` <br />`是一个空元素，通常用来被实现换行的操作

## html文本格式化
文本格式化标签：
```
<b>加粗文本
<i>斜体文本
<em>斜体文本
<strong>重要文本，效果与<b>相似
<big>字体放大
<samll>缩小文本
<sub>下标
<sup>上表
<del>删除子
<ins>插入字
```

计算机输出标签:
```
<pre>预格式文本：在pre内的文本会保留空格和换行符，文本会呈现为等宽字体
```

特色标签：
```
<address>地址
<abbr>表示一个缩写词或者首字母缩略词，在鼠标移到文字上会显示详细信息
<bdo>文字方向：属性dir=="ltr"/"rtl"
<blockquote> 长引用，通常会进行缩进
<q> 短引用，通常周围会插入引号
<cite>书籍等的标题

```
# html头部
`<head>`指定了所有的头部标签元素，在`<head>`中可以添加的元素标签有：
` <title>, <style>, <meta>, <link>, <script>, <noscript>, <base>.`
- title定义了
	- 定义了浏览器工具栏的标题
	- 当网页添加到收藏夹时，显示在收藏夹中的标题
	- 显示在搜索引擎结果页面的标题
- `<base>` 标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接:
- `<style>` 标签定义了HTML文档的样式文件引用地址.在`<style>` 元素中你也可以直接添加样式来渲染 HTML 文档；
- `<link>`标签定义了文档与外部资源之间的关系。标签通常用于链接到样式表:
- meta标签描述了一些基本的元数据。
 
 ```
 为搜索引擎定义关键词:
 <meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
 为网页定义描述内容:
 <meta name="description" content="免费 Web & 编程 教程">
 定义网页作者:
 <meta name="author" content="Runoob">
 每30秒钟刷新当前页面:
 <meta http-equiv="refresh" content="30">
 ```
 # html表格
 表格由`<table>`定义，每个表格的若干行由`<tr>`定义，每行被分为若干单元格，由`<td>`定义
 `<table>`有border属性，如果不定义边框属性，表格将不显示边框。
 表格的表头使用 `<th>` 标签进行定义。大多数浏览器会把表头显示为粗体居中的文本.
 `<caption>`定义表格标题
 `<colgroup>` 标签用于对表格中的列进行组合，以便对其进行格式化。
 `<col> `标签规定了 `<colgroup>` 元素内部的每一列的列属性。
 `<thead>` 元素应该与 `<tbody>` 和 `<tfoot>` 元素结合起来使用，用来规定表格的各个部分（表头、主体、页脚）
 ## 多行表格
 若是多列，使用colspan="数字"进行跨列
 若是多行，使用rowspan="数字"进行跨行
 这两个都是`<th>`的属性
# html列表
## 无序列表
无序列表是一个项目的列表，此列项目使用粗体圆点进行标记。
```
<ul>
	<li>1</li>
	<li>2</li>
</ul>
```
## 有序列表
有序列表也是一列项目，列表项目使用数字进行标记。
```
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```
## 自定义列表
自定义列表以 `<dl>` 标签开始。每个自定义列表项以` <dt> `开始。每个自定义列表项的定义以 `<dd>`开始。
```
<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
</dl>
```
# 区块
HTML `<div>` 元素是块级元素，它可用于组合其他 HTML 元素的容器。浏览器会在其前后显示折行
`<div>` 元素的另一个常见的用途是文档布局。
HTML `<span> `元素是内联元素，可用作文本的容器,<span> 元素也没有特定的含义。

# HTML表单
表单是一个包含表单元素的区域。表单元素是允许用户在表单中输入内容。表单使用表单标签`<form>`来设置：
```
<form>
input元素
</form>
```
## HTML表单
多数情况下被用到的表单标签是输入标签`<input>`，输入类型是由`type`定义的，经常用到的输入类型有：
- 文本域(Text Fields)
	`<input type="text">`
	在大多数浏览器，文本域的默认宽度是 20 个字符。
- 密码字段
	`<input type="password">`
	密码字段字符不会明文显示，而是以星号或圆点替代。
- 单选按钮(Radio Buttons)
	`<input type="radio">`
- 复选框
   `<input type="checkbox">`
    用户需要从若干给定的选择中选取一个或若干选项。
 - 提交按钮(Submit Button)
	 `<input type="submit"> `
	 当用户单击确认按钮时，表单的内容会被传送到另一个文件。表单的动作属性定义了目的文件的文件名。由动作属性定义的这个文件通常会对接收到的输入数据进行相关的处理。
	 
	 ```
	 <form name="input" action="html_form_action.php" method="get">
Username: <input type="text" name="user">
<input type="submit" value="Submit">
</form>
	 ```
## 其他元素
- `<textarea>` 标签定义一个多行的文本输入控件。
	文本区域中可容纳无限数量的文本，其中的文本的默认字体是等宽字体.
	可以通过 cols 和 rows 属性来规定 textarea 的尺寸大小，不过更好的办法是使用 CSS 的 height 和 width 属性。
- `<label> `标签为 input 元素定义标注（标记）。
- `<fieldset>` 标签可以将表单内的相关元素分组。
`<fieldset>` 标签会在相关表单元素周围绘制边框。
`<legend> `标签为` <fieldset>` 元素定义标题
- `<select>` 元素用来创建下拉列表。

 `<select>` 元素中的 `<option>` 标签定义了列表中的可用选项。
- `<option>`选项使用value属性定义option的值，
 ```
 <select>
  <option value="volvo">Volvo</option>
  <option value="saab">Saab</option>
  <option value="opel">Opel</option>
  <option value="audi">Audi</option>
</select>
 ```
- `<optgroup> 标签经常用于把相关的选项组合在一起。`
 ```
 <select>
  <optgroup label="Swedish Cars">
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
  </optgroup>
  <optgroup label="German Cars">
    <option value="mercedes">Mercedes</option>
    <option value="audi">Audi</option>
  </optgroup>
</select>
 ```
- `<datalist>` 标签规定了 `<input>` 元素可能的选项列表。
```
<input list="browsers">
<datalist id="browsers">
  <option value="Internet Explorer">
  <option value="Firefox">
  <option value="Chrome">
  <option value="Opera">
  <option value="Safari">
</datalist>
```
- `<output>` 标签作为计算结果输出显示(比如执行脚本的输出)。
	其属性for描述计算中使用的元素与计算结果之间的关系。
	```
	<form oninput="x.value=parseInt(a.value)+parseInt(b.value)">0
  <input type="range" id="a" value="50">100
  +<input type="number" id="b" value="50">
  =<output name="x" for="a b"></output>
</form>
	```
	# HTML框架
	```
	<iframe src="URL"></iframe>
	```
	使用该语法可以在本页面展示不同的其他页面，地址为url
	长宽属性：
	```
	<iframe src="demo_iframe.htm" width="200" height="200"></iframe>
	```
	边框属性，在frameborder属性值为0时移除边框：
	```
	<iframe src="demo_iframe.htm" frameborder="0"></iframe>
	```
	iframe可以显示一个目标链接的页面,目标链接的属性必须使用iframe的属性，如下实例:	
	
	```
	<iframe src="demo_iframe.htm" name="iframe_a"></iframe>
	<p><a href="http://www.runoob.com" target="iframe_a">RUNOOB.COM</a></p>
	```
