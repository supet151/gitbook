# C8-BOM
ES是JS的核心，在Web中使用JS时，BOM(浏览器对象模型)是真正的核心。
BOM提供了很多对象用于访问浏览器的功能。

## window对象
BOM的核心对象是window，它表示浏览器的一个实例。在浏览器中，window是通过JS访问浏览器窗口的一个接口，也是ES规定的Global对象。

在网页中定义的任何一个对象、变量和函数，都以window作为其Global对象，因此有权访问parseInt()等方法。

### 全局作用域
window扮演着ES中Global对象的角色，因此所有在全局作用域中声明的变量、函数都会变成window对象的属性和方法。

定义全局变量与在window对象上直接定义属性有差别：
全局变量不能通过delete操作符删除，而直接在window对象上定义的属性可以。

使用var语句添加的window属性有一个名为`[[Configurable]]`的特性，这个特性的值被设置为`false`，因此这样定义的属性不可以通过delete操作符删除。

尝试访问未声明的变量将会导致错误，但是通过查询window对象，可以知道某个可能未声明的变量是否存在。
```
//抛出错误，因为oldValue未定义
var newValue=oldValue;
//不抛出错误，得到undefined
var newValue=window.oldValue;
```

### 窗口关系及框架
H**TML5不支持`frameset`&`frame`**
页面中包含框架时，每个框架都会拥有自己的window对象，并且保存在frames集合中。

在frames集合中，可以使用数值索引或者框架名称来访问相应的window对象。
每个window对象都有一个name属性，其中包含框架的名称。

```
<html>
    <head>
        <title>Frameset Example</title>
    </head>
    <frameset rows="160,*">
        <frame src="www.baidu.com" name="topFrame">
        <frameset cols="50%,50%">
            <frame src="www.sina.com" name="leftFrame">
            <frame src="qq.com" name="rightFrame">
        </frameset>
    </frameset>
</html>
```
对于上方的框架，可以使用：
- window.frames[0]
- window.frames["topFrame"]

最好使用后者

top对象始终执行最高(最外层)的框架，即浏览器窗口。
使用它可以确保在一个框架中正常地访问另一个框架。

在一个框架中，window对象指向的都是框架的特定实例(当前框架)。

parent也是window的一个属性。这个对象将会始终指向当前框架的直接上层框架。
在某些情况下，parent可能等于top；但是在没有框架的情况下，parent一定等于top，且它们都等于window。

除非最高层窗口是通过`window.open()`打开的，否则其window对象的name属性不会包含任何值。

`self`对象将会始终指向window。

在使用框架的情况下，浏览器中会存在多个Global对象。在每个框架中定义的全局变量会自动成为框架中window对象的属性。由于每个window对象都包含原生类型的构造函数，因此每个框架都有自己的一套构造函数，这些构造函数并不相等。这将会影响到对于跨框架传递的对象使用`instanceof`操作符。

### 窗口位置
用于确定和修改window对象位置的属性和方法有很多。各个浏览器提供了自己支持的属性。

使用下列代码可以跨浏览器取得窗口左边和上边的位置： 
```
var left=(typeof window.screenLeft=="number")?
	window.screenLeft:window.screenX;
var topPos=(typeof window.screenTop=="number")?
	window.screenTop:window.screenY;
```

这个例子运用二元操作符来确定属性值。

`moveTo`&`moveBy()`方法能够精确将窗口移动到一个新位置。
前者接收新位置的x和y坐标值；
后者接收水平和垂直方向上移动的像素数。

这两个方法可能被浏览器禁用且两个方法都不适用于框架，只能对最外层的window对象使用。

### 窗口大小
大部分浏览器使用`innerWidth`,`innerHeight`,`outerWidth`,`outerHeight`
其中，前面两个属性将会返回容器中页面视图区的大小(减去边框宽度)。
后面两个属性将会返回浏览器窗口本身的尺寸。

Chrome中，两组属性将会返回相同的值，即视口大小而非浏览器窗口大小。

部分浏览器中，使用
`document.documentElement.clientWidth`&`document.documentElement.clientHeight`保存了页面视口的信息。

在标准模式下，IE6能获得以上的属性值。
在混杂模式下，需要通过`document.body.clientWidth`&`document.body.clientHeight`取得相同的消息。
混杂模式下的Chrome，通过两组属性都可以获得视口大小。


对于移动设备，`window.innerWidth`&`window.innerHeight`保存着可见视口。
移动IE浏览器使用`document.body.clientWidth`&`document.body.clientHeight`提供了相同的信息。这些值将不会随着页面缩放变化。

使用`resizeTo()`&`resizeBy()`方法可以调整浏览器窗口的大小。这两个方法接收两个参数。
前者接收浏览器的新宽度和新高度。
后者接收新窗口和原窗口的大小差值。

这两个方法也有可能被浏览器禁用；这两个方法只能对最外层的window对象使用。


### 导航和打开窗口
`window.open()`可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。

这个方法可以接收4个参数：
- 要加载的URL
- 窗口目标
- 一个特性字符串
- 一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值

通常只传递一个参数，最后一个参数只在不打开新窗口的情况下使用。

如果为该方法传递了第二个参数，而且该参数是已有窗口或框架的名称，那么就会在具有该名称的窗口或框架中加载第一个参数指定的URL。

```
window.open("http://www.wrox.com/",topFrame)
//equal:
<a href="http://www.wrox.com" target="topFrame"></a>
```
调用该方法，如果有一个名叫`topFrame`的窗口或者框架，就会在该窗口或者框架加载这个url；
否则会创建一个新窗口并命名为`topFrame`。
第二个参数也可以是：`_self_`,`_parent_`,`_top`,`_blank`

####弹出窗口
如果给`window.open()`传递的第二个参数并不是一个已经存在的窗口或框架。
将会依据第三个参数位置上传的字符串创造一个新窗口或者新标签页。

如果没有传入第三个参数，将会打开一个带有全部默认设置的新浏览器窗口或者新标签页。
不打开新窗口的情况下，会忽略第三个参数。

第三个参数是一个逗号分隔的设置字符串，表示在新窗口中显示哪些特征。
[特征](https://www.w3school.com.cn/jsref/met_win_open.asp#windowfeatures)

字符串中的名值对使用等号表示，且正则特性字符串不允许出现空格。

`window.open()`将会返回一个指向新窗口的引用。引用的对象与其他window对象大致相似，但是允许进行更多控制。

使用`新窗口的引用.close()`方法可以关闭新打开的窗口。
这个方法仅仅适用于使用`window.open()`打开的弹出窗口。浏览器的主窗口不能使用该方法进行关闭。

弹出窗口也可以调用`top.close()`在不经用户允许的情况下关闭自己。窗口关闭后，窗口的引用还在，可以检测其`closed`属性。
```
//引用wroxWin
wroxWin.close();
alert(wroxWin.closed);//true
```

新创建的window对象有一个`opener`属性，其中保存着打开它的原始窗口对象。
这个属性只在弹出窗口的最外层中有定义，且指向调用`window.open()`的窗口或者框架。

值得注意的是，原始窗口中没有指向自身打开的弹出窗口的指针。
窗口不会跟踪记录它们打开的弹出窗口，需要时需要自己手动实现。

有些浏览器会在独立的进程中运行每个标签页。当一个标签页打开另一个标签页的时候，如果两个window之间需要彼此通信，那么新标签页就不能运行在独立的进程中。

Chrome中，将新创建的标签页的`opener`属性设置为`null`，即在单独的进程中运行新标签页。这将会切断标签页之间的联系，该联系一但切断，无法恢复。

#### 安全限制
不同的浏览器出于安全的考虑，对于弹出窗口进行了限制。
如不允许在在屏幕之外创建弹出窗口、不允许将弹出窗口移动到屏幕外、不允许关闭状态栏、不允许关闭地址栏、默认情况下不允许移动弹出窗口或者调整其大小。

有的浏览器只根据用户操作来创建弹出窗口。
在页面加载完成前，`window.open()`将不会执行，而且还可能会将错误的信息显示给用户。

换句话说，只能通过单击或者击键来打开弹出窗口。

#### 弹出窗口屏蔽程序
弹出窗口屏蔽程序将会将绝大多数用户不想看到的弹出窗口屏蔽。
在屏蔽弹出窗口时，可能是浏览器的内置屏蔽程序阻止弹出窗口，那么`window.open()`很可能会返回`null` 。
只要检测这个返回值就可以确定弹出窗口是否被屏蔽。
```
var wroxWin=window.open("http://www.wrox.com","_blank");
if(wroxWin==null){
	alert("the popup was blocked!");
}
```

如果时浏览器扩展或其他程序阻止弹出窗口，那么`window.open()`通常会抛出一个错误。
使用下面的例子来准确检测弹出窗口是否屏蔽：
```
var blocked=false;

try{
	var wroxWin=window.open("http://www.wrox.com","_blank");
		if(wroxWin==null){
			blocked=true;
		}
}catch(ex){
	blocked=true;
}
if(blocked){
	alert("The popup was blocked!");
}
```
在任何情况下，以上代码都可以检测出调用`window.open()`打开的弹出窗口是不是被屏蔽了。

值得注意的是，屏蔽弹出窗口不会阻止浏览器显示与被屏蔽的弹出窗口有关的消息。

### 间歇调用和超时调用
JS是单线程语言，但它允许通过设置超时值和间歇时间来调度代码在特定的时刻执行。
前者是在指定的时间后执行代码，后者是每隔一定时间就执行一次代码。

超时调用需要使用window对象的setTimeout()方法，它接收两个参数：要执行的代码和以毫秒表示的时间。
第一个参数可以是一个包含JS代码的字符串，也可以是一个函数。但是**一般情况下应该传入函数，传入字符串可能会导致性能损失**。
```
setTimeout(function{
	alert("hello!")
},1000);
```
**JS任务队列**
虽然第二个参数规定了等待的时间，但是**经过该时间后，指定的代码不一定会执行**。
JS是一个单线程序的解释器，因此只能一定时间执行一段代码。为了控制要执行的代码，就需要一个任务队列。这些任务会按照它们添加到队列中的顺序执行。

上面方法的第二个参数将会告诉JS再过多长时间把当前任务添加到队列中。
如果队列为空，添加的代码会立刻执行，如果不为空，会等待队列前面的代码执行结束。

调用结束后，对应方法会返回一个数值ID，表示超时调用，这个ID是计划执行代码的唯一标识符，可以通过它来取消超时调用计划。
```
var timeoutId=setTimeout(function{
	alert("hello!")
},1000);
//Cancel
clearTimeout(timeoutId);
```

只要是在指定的时间尚未过去前重新调用`clearTimeout()`，就可以完全取消超时调用。

*超时调用的代码都是在全局作用域内执行的，因此函数中this的值在非严格模式下指向window对象，在严格模式下为undefined。*

间歇调用和超时调用类似，只不过会按照指定的时间间隔重复执行代码，直至间歇调用被取消或者页面被卸载。
方法：
`setInterval()`
传入参数与`setTimeout()`相同。

调用该方法也会返回一个间歇调用ID，该ID可用于在将来某个时刻取消间隔调用。
取消间歇调用使用`clearInterval()`。

取消间歇调用的重要性要远远高于取消超时调用，因为在不加干涉的情况下，间歇调用会一直持续执行到页面卸载。
我们可以使用下面的例子替代间歇调用:
```
var num=0;
var max=10;

function incrementNumber(){
	num++;
	//如果未到达指定值，设置超时调用
	if(num<max){
		setTimeout(incrementNumber,500);
	}else{
		alert("Done!");
	}
}
setTimeout(incrementNumber,500);
```
在使用超时调用时，不需要跟踪超时调用ID，每次代码执行后，如果不设置下一次，调用会自动停止。
***一般情况下，使用超时调用来模拟间歇调用是一种最佳模式。***
在开发环境下，很少使用真正的间歇调用，原因是后一个间歇调用可能会在前一个间歇调用结束前启动。
使用超时调用可以避免这个问题，所以**应该尽量避免使用间歇调用**。

### 系统对话框
浏览器使用`alert()`,`confirm()`,`prompt()`方法可以调用系统对话框向用户展示信息。

这些系统对话框的外观由操作系统&浏览器设置。
通过这几个方法打开对话框都是同步和模态的。即：显示这些对话框时，代码会停止运行，在关掉对话框后恢复。

`alert()`用于产生一些用户无法控制的信息，如错误消息，只包含一个确认按钮。

`confirm()`用于产生确认对话框，将会显示除了确认按钮外的取消按钮。
```
if(confirm("are you sure?")){
	alert("you are sure!");
}else{
	alert("you are not sure!");
}
```
`prompt`用于产生一个提示框，提示用户输入一些文本，会显示一个文本输入域、OK&Cancel按钮。
该方法接收两个参数，显示给用户的文本提示和文本输入域的默认值，第二个参数可为空字符串。
```
var result=prompt("what is your name?","");
if(result!==null){
	alert("welcome,"+result);
}
```

这些系统对话框适合向用户展示信息和请用户做出决定。

Chrome也引入了一种新特性，如果当前脚本在执行过程中会打开多个对话框，从第二个对话框开始，每个对话框中会显示一个复选框，以便用户阻止后续的对话框显示，除非用户刷新页面。

浏览器会在空闲的时候重置对话框计数器，如果两次独立的用户操作分别打开两个警告框，这两个警告框都不会显示复选框；
如果一个用户操作产生两个警告框，第二个警告框中会显示复选框。

JS也可以打开**查找和打印**对话框，两个对话框都是**异步**的(对话框计数器不会将其计算在内，不受用户禁用后续的影响)，能够将控制权立刻交还给脚本。
这两个对话框和使用浏览器的对应功能相同。
```
//打印
window.print();
//查找
window.find();
```
*在Chrome里实验，查找框似乎没有生效？*

这两个方法不会反馈用户的操作，所以用处有限。

## location对象
[文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Location)
location提供了当前窗口中加载的文档有关的信息，还提供了一些导航功能。

location既是window对象的属性，又是document对象的属性；
即：`window.location`与`document.location`引用同一个对象。

location对象保存着当前文档的信息，也可以将URL解析为独立的片段，让开发人员可以通过不同的属性访问这些片段。
### 查询字符串参数
可以访问URL包含的查询字符串的属性。
`location.search()`返回从问号到URL末尾的所有内容，但没有办法逐个访问其中的每个查询字符串参数。

使用下面的例子，解析查询字符串，返回包含所有参数的一个对象：
```
function getQueryStringArgs(){
	//取得查询字符串并去掉开头的问号
	var qs=(location.search.length>0?location.search.substring(1):""),
	//保存数据的对象
	args={};
	
	items=qs.length?qs.split("&"):[];
	item=null;
	name=null;
	value=null;
	//在for循环中使用
	i=0;
	len=items.length;
	for(i=0;i<len;i++){
		item=items[i].split("=");
		name=decodeURLComponent(item[0]);
		value=decodeURLComponent(item[1]);
		if(name.length){
			args[name]=value;
		}
	}
	return args;
}	
```
每个查询字符串参数都成了返回对象的属性。这样极大地方便了每个参数的访问。

### 位置操作
使用location对象可以通过很多方式改变浏览器的位置。
首先，就是使用`assign()`方法并为其传递一个URL。
```
location.assign("http://www.wrox.com");
```
这样，就可以立刻打开新URL并在浏览器的历史记录中生成一条记录。
如果是将`location.href`||`window.location`设置一个URL值，也会以该值调用assign()方法。

在这些改变浏览器位置的方法中，**最常用的是设置`location.href`属性。**

另外，修改location对象的其他属性也可以改变当前加载的页面。
如：`hash`,`search`,`hostname`,`pathname`,`port`属性设置为新值来改变URL。

使用以上的属性中的任何一种修改URL之后，浏览器的历史记录将会生成一条新记录，因此用户可以通过点击**后退**按钮都会导航到前一个页面。
要禁用这种行为，可以使用`replace()`方法。
只接受一个参数，导航到的URL地址。

浏览器位置改变，但是不会在历史记录中生成新记录。在调用replace()方法之后，用户不能回到前一个页面。

`reload()`：重加载当前页面。
无参数时，自动选取最有效的方式加载(浏览器缓存或者服务器)
传递`true`，强制从服务器重新加载
该方法之后的代码可能不执行，最好将其作为最后一行。

## navigator对象
识别客户端
### 检测插件
检测是否安装了一组特定插件：
插件信息存放在plugins数组中
- name:插件名
- description:插件描述
- filename:插件文件名
- length:插件所处理的MIME类型数量

```
function hasPlugin(name){
    name=name.toLowerCase();
    for(var i=0;i<navigator.plugins.length;i++){
        if(navigator.plugins[i].name.toLowerCase().indexOf(name)>-1){
            return true;
        }
    }
    return false;
}
```

每个插件对象本身时一个`MimeType`的数组，这些对象可以通过方括号语法访问。
MimeType有4个属性：
- `description`包含MIME类型描述
- `enablePlugin`回指插件对象
- `suffixes`表示与MIME类型对应的文件扩展名字符串，以逗号分隔
- `type`表示完整MIME类型字符串

IE的插件检测：
IE不支持NetScape插件，要使用专有的`ActiveXObject`类型进行检测并尝试创建一个特定插件的实例。

```
function hasIEPlugin(name){
    try{
        new ActiveXObject(name);
        return true;
    }catch(ex){
        return fales;
    }
}
```

上例中，函数接收一个**COM标识符**作为参数。

实际检测时，应该对每个插件分别创建检测函数。
```
function hasFlash(){
    var result=hasPlugin("Flash");
    if(!result){
        result=hasIEPlugin("ShockwaveFlash.ShowwaveFlash");
    }
    return result;
}
```

`refresh()`是`plugins`集合中的方法，用于刷新`plugins`以反映最新安装的插件。
接收一个布尔值参数，为true时，将会重新加载包含插件的所有页面；
否则只更新`plugins`

### 注册处理程序

## screen对象
表明客户端的能力，包括浏览器窗口外部的显示器信息。

这些信息经常出现在测定客户端能力的站点跟踪工具中，但是不会影响其功能。

```
window.resizeTo(screen.availWidth,screen.availHeight);
```
上面的例子改变了浏览器窗口大小，使其占满屏幕，该例子不一定在所有环境下有效。

## history对象
history对象保存着用户的上网记录。
每个浏览器窗口、标签、框架都有自己的history与特定的window对象关联。
开发人员无法获得用户浏览的URL，通过用户访问过的页面列表实现后退和前进。

`go()`方法在用户的记录中跳转。
```
history.go(-1)//后退一页
history.go(1)//前进一页
history.go(2)//前进两页
```
传递字符串时，浏览器将跳转到历史记录中包含该字符串的第一个位置，未找到时，什么也不做。

`history.back()`后退
`history.forward()`前进

`length`属性保存历史记录的数量。
对于加载到窗口、标签页或框架中的第一个页面而言，该属性为0。
