# C7-函数表达式

定义函数有两种方式：

* 函数声明
* 函数表达式

函数声明即：

```text
function functionName(arg0,arg1,arg2){
    //函数体
}
```

部分浏览器提供了非标准的`name`属性以访问函数的指定名字。这个属性的值永远等于function后的关键字标识符。

**函数声明提升** 在执行代码前，将会先读取**函数声明**。这意味着可以把函数声明放在调用其的语句后边。

函数表达式即：

```text
var functionName=function(arg0,arg1,arg2){
    //函数体
}
```

这种形式将创建一个函数并且将它赋值给变量。 这种情况下创建的函数叫做**匿名函数**\(拉姆达函数\)，因为function后没有标识符。

函数表达式必须在使用前赋值，因为其不存在变量提升。

**永远不要试图**：

```text
if(condition){
    function sayHi(){
        alert("hi!");
    }
}else{
    function(){
        alert("aHa!");
    }
}
```

在ES中，上面的代码是无效的，而且浏览器的修正并不统一。这种方式很危险。

但是可以改写成：

```text
var sayHi;
if(condition){
    sayHi=function(){
        alert("Hi!");
    };
}else{
    sayHi=function(){
        alert("aHa!");
    };
}
```

上面的例子将会正常执行。

而且，在把函数当成值的时候，都可以使用匿名函数。

## 递归

递归即在一个函数内部通过名字调用自身的情况下构成的。 如：

```text
function factorial(num){
    if(num<=1){
        return 1;
    }else{
        return num*factorial(num-1);
    }
}
```

上面的递归函数面临的问题是：

```text
var anotherFactorial=factorial;
factorial=null;
alert(anotherFactorial(4));
```

在上面的代码中，调用`anotherFactorial()`需要执行内部的函数引用`factorial`,然而该函数已经变成了`null`，所以会产生错误的结果。

可以使用`arguments.callee`指向一个正在执行的函数，用其可以实现函数的递归调用。 如：

```text
function factorial(num){
    if(num<=1){
        return 1;
    }else{
        return num*arguments.callee(num-1);
    }
}
```

这样就可以防止之前的问题出现，因此，应该使用这种方法编写递归函数。

严格模式下，不能通过脚本访问`arguments.callee`，访问这个属性会导致错误，但是可以使用命名函数达到相同的结果。

```text
var factorial=(function f(num){
    if(num<=1){
        return 1;
    }else{
        return num*f(num-1);
    }
});
```

以上代码创建了一个名为f\(\)的命名函数表达式，这样函数名f将在任何情况下保持有效，保证递归正常完成。该方法可以在严格和非严格模式下使用。

## 闭包

闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包的常见方式是在函数内部创建一个另一个函数。

如:

```text
function createHifunction(msg){
    return function(){
        alert("hi!",msg);
    }
}
```

在上面的代码中，返回的内部函数访问了外部函数中的变量。因为内部函数的作用域链中包含外部函数的作用域。

当某个函数被第一次调用时，会创建一个**执行环境**以及相应的**作用域链**，并把作用域链赋值给一个特殊的内部属性\(`[[Scope]]`\)。之后将会使用`this`&`arguments`和其他命名参数的值来初始化函数的**活动对象**。在作用域链中，外部函数的活动对象始终处于第二位，外部函数的外部函数的活动对象处于第三位，直到作用域链终点的全局执行环境。

在函数执行过程中，将会在作用域链内查找变量。

后台的每个执行环境都有一个_表示变量的对象_：**变量对象**。全局环境的变量对象始终存在，函数的局部环境的变量对象只在函数执行过程中存在。

在创建一个函数f时，将会首先创建一个预先包含全局变量对象的作用域链，这个作用域链被保存在内部的`[[Scope]]`属性中。 在调用一个函数时，将会为函数创建一个执行环境，然后复制函数的`[[Scope]]`属性中的对象构建起执行环境作用域链的前端。

本质上作用域链是一个指向变量对象的指针列表，它只引用但不实际包含变量对象。

在函数访问一个变量时，将会从作用域链中搜索具有相应名字的变量。 **通常**，函数执行完毕后，局部活动对象就会被销毁。内存中仅保留全局作用域。

但是闭包时，在外部函数中定义的函数会将包含外部函数的活动对象添加到自身的作用域链中。

```text
function createHifunction(msg){
    return function(){
        alert("hi!",msg);
    }
}
```

在上面的例子中，匿名函数从`createHifunction`中被返回之后，它的作用域链被初始化为包含其的外部函数的活动对象和全局变量对象。 这样匿名函数就可以访问在包含\(它的\)函数中定义的所有变量。且在其包含函数执行完毕后，包含函数的活动对象也不会销毁，因为匿名函数的作用域链仍然在引用这个活动对象。

**当外部函数返回后，其执行环境的作用域链会被销毁，但它的活动对象会保留在内存中。直到匿名函数被销毁，其外部函数的活动对象才会被销毁。**

为了解除对匿名函数的引用，我们应该使用显示`=null`进行赋值。

闭包会携带包含它的函数的作用域，会比其他函数占用更多内存。过度使用闭包可能会导致内存占用过多，应该只在完全必要的时候再使用闭包。

### 闭包与变量

作用域链的配置导致闭包只能取得包含函数中任何变量的最后一个值，如：

```text
function createFunction(){
    var result=new Array();
    for(var i=0;i<10;i++){
        result[i]=function(){
            return i;
        }
    }
}
```

这个函数将会返回一个函数数组。 从表面上来看，似乎每个函数都应该返回自己的索引值。但是实际上，每个函数都会返回10，因为每个函数的作用域链中都保存着包含函数的活动对象，且它们引用的是**同一个**变量i。 在外部函数返回函数数组之后，变量i的值为10，所以每个函数都引用着保存变量i的同一个变量对象，所以每个函数都会返回相同的i值。

可以通过创建另一个匿名函数强制让闭包的行为符合预期。

```text
function createFunction(){
    var result=new Array();

    for(var i=0;i<10;i++){
        result[i]=function(num){
            return function(){
                return num;
            }
        }(i);//将会立即执行并且传入i值
    }
    return result;
}
```

### 关于this对象

在闭包中使用this对象可能会导致问题。 在全局函数中，this等于window，而当函数被作为某个对象的方法调用时，this等于那个对象。

**匿名函数的执行环境具有全局性，所以其this对象通常指向window。** 如：

```text
var name="window";
var obj={
    name:"my obj";
    getNameFun:function(){
        return function(){
            return this.name;
        }
    }
}
alert(object.getNameFun()());
```

在上面的例子中，将会返回提示window字符串，即全局变量name的值。

这是因为函数在调用的时候，其活动对象会自动获得两个特殊变量:this&argument。内部函数在搜索这两个变量时，只会搜索到其活动对象为止。所以不能访问外部的这两个变量。

但是如果这样书写：

```text
var name="window";
var obj={
    name:"my obj";
    getNameFun:function(){
        var that=this;
        return function(){
            return that.name;
        }
    }
}
alert(object.getNameFun()());
```

上面的例子将会返回"my obj"，因为在定义匿名函数之前，使用了that变量保存了this对象。

`this`和`arguments`存在着同样的问题，如果想要访问作用域中的arguments对象，必须将该对象的引用保存在另一个闭包能够访问的变量中。

### 内存泄露

如果闭包的作用域内保存着一个HTML元素，就意味着该元素将无法被销毁。

```text
function assignHandler(){
    var element=document.getElementBy("someElement");
    element.onclick()=function(){
        alert(element.id);
    }
}
```

在上面例子中的闭包中，创建了一个循环引用。 由于匿名函数保存了一个对于assignHandler的活动对象的引用，就会导致element无法减少引用次数。只要匿名函数存在，element的引用次数至少为1，它所占用的内存不可能被回收。

如果：

```text
function assignHandler(){
    var element=document.getElementBy("someElement");
    var id=element.id;
    element.onclick()=function(){
        alert(id);
    }
    element=null;
}
```

通过改变书写方式，将element.id的一个副本保存，在闭包中引用该变量就可以消除循环引用。 之后手动将element变量设为`null`，解除对于DOM的引用，确保能够正常回收占用的内存。

## 模仿块级作用域

JS中没有块级作用域的概念，所以在块语句中定义的变量，实际上实在包含函数中创建的。 因此，可以在for循环外部访问for循环中新声明的变量。

这样，如果后面重复声明变量，JS将会忽略后续声明，只执行初始化等操作。

使用匿名函数可以模仿块级作用域以解决这个问题:

```text
(function(){
    //块级作用域
})();
```

上面的匿名函数包括在一个圆括号中，表示是一个函数表达式。紧随其后的`()`将其变成立刻执行函数。

function声明外的括号很重要，加上括号后，函数声明会变成一个函数表达式，函数表达式后可以跟圆括号，而函数声明后不可以跟圆括号。

在全局作用域中，使用这种技术可以避免向全局作用域中添加太多的变量和函数。

_使用这种方法也可以减少闭包占用内存的问题，没有指向匿名函数的引用，所以函数执行完毕之后就会立即销毁其作用域。_

## 私有变量

JS中没有私有成员的概念，所有的对象属性都是公有的；但是有私有变量的概念。任何在函数中定义的变量都可以被认为是私有变量，因为不能再函数的外部访问这些变量。 私有变量包括函数参数、局部变量和在函数内部定义的其他函数。

如果在一个函数内部创建一个闭包，闭包通过自己的作用域链可以访问到这些变量。利用这个特性，可以创建用于访问私有变量的公有方法。

有权访问私有变量和私有函数的公有方法也被称为**特权方法**。

有两种在对象上创建特权方法的方式：

* 在构造函数中定义特权方法

```text
function MyObject(){
    var privateVariable=10;
    function privateFunction(){
        return false;
    }
    this.publicMethod=function(){
        privateVariable++;
        return privateFunction();
    };
}
```

在这个模式中，在构造函数内部定义了所有私有变量和函数。然后又继续创建了能够访问这些私有成员的特权方法。

利用私有和特权成员，可以隐藏那些不应该被直接修改的数据，如：

```text
function Person(name){
    this.getName=function(){
        return name;
    }
    this.setName=function(){
        name=value;
    }
}
var person=new Person("Mike");
alert(person.getName());//"Mike"
person.setName("John");
alert(person.getName());"Greg"
```

以上代码的构造函数中定义了两个特权方法，通过这两个特权方法可以在实例外更改实例的私有变量。

使用构造函数定义特权方法的缺点是必须使用构造函数模式来达到这个目的。 构造函数模式会对每个实例创建一组新方法。

### 静态私有变量

通过在私有作用域中定义私有变量或者函数，同样可以创建特权方法。

```text
(function(){
    //私有变量和私有函数
    var privateVariable=10;
    function privateFunction(){
        return false;
    }
    //构造函数
    MyObject=function(){
    };
    //公有/特权方法
    MyObject.prototype.publicMethod=function(){
        privateVariable++;
        return privateFunction();
    }
})();
```

这个模式在私有作用域中封装了一个构造函数及其相应的方法。公有方法实在原型上定义的，体现了典型的原型模式。 这个模式在定义构造函数时没有使用函数声明，而是使用函数表达式。

**函数声明只能创建局部函数。** 同时，在声明MyObject时也没有使用var，这使得该变量成为一个全局变量，能够在私有作用域外被访问到。这种写法在严格模式下会导致错误。

**这种模式下，私有变量和函数由实例共享。**特权方法在原型上定义，所有实例使用同一个函数，同时特权方法作为一个闭包，保留着对于包含作用域的引用。

这种模式下，通过原型增加了代码复用，但是每个实例都没有自己的私有变量。

多查找作用域链中的一个层次会影响查找速度，这也是使用闭包和私有变量的一个明显的不足。

### 模块模式

模块模式是为单例创建私有变量和特权方法。所谓单例，是指只有一个实例的对象。 JS使用字面量的方式创建单例对象。

模块模式通过为单例添加私有变量和特权方法使其能够得到增强。

```text
var singleton=function(){
    //私有变量和私有函数
    var privateVariable=10;

    function privateFunction(){
        return false;
    }
    //初始化方法
    //...

    //特权/公有方法和属性
    return {
        publicProperty:true,
        publicMethod:function(){
            privateVariable++;
            return privateFunction();
        }
    }
}();
```

该模块模式使用了一个返回对象的匿名函数。在这个匿名函数内部，首先定义了私有变量属性和函数。然后将一个字面量对象为函数的值返回。 该对象在匿名函数内部定义，有权访问私有变量。

这个对象字面量定义的就是单例的公共接口。 该模式在需要对单例进行某些初始化，又需要维护其私有变量时是非常有用的。

在Web应用程序中，经常需要使用一个单例来管理程序级的信息。 如果需要必须创建一个对象并以某些数据对其进行初始化，同时还要公开一些能够访问这些私有数据的方法，就可以使用模块模式。 该模式创建的每个单例都只是Object的实例，因为最后要通过一个字面量对象来表示它。

### 增强的模块模式

增强的模块模式将会在返回对象之前加入对其增强的代码。 这种增强的模块适合那些**单例必须是某种类型的实例**，同时还必须添加某些属性对其进行增强的情况。

```text
var singleton=function(){
    //私有变量和私有函数
    var privateVariable=10;
    function privateFunction(){
        return false;
    }
    //初始化方法
    //...
    //创建对象
    var object=new CustomType();
    //添加特权/公有属性和方法
    object.publicProperty=true;
    object.publicMethod=function(){
            privateVariable++;
            return privateFunction();
    };
    //返回
    return object;

}();
```

在这个例子中，定义了一个`CustomType`对象的局部变量版实例。

