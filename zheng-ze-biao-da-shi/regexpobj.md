# 正则表达式&JavaScript's RegExp对象

来源：[阮一峰-JavaScript标准参考教程](https://javascript.ruanyifeng.com/stdlib/regexp.html#toc10)

## 正则表达式的匹配规则

### 字面量与元字符

字面量即在正则表达式中表示自己字面含义的字符 元字符有特殊的含义，不代表字面的值

#### 点字符\(.\)

点字符会匹配除了回车`\r`,换行`\n`,行分隔符`\u2028`,段分隔符`\u2029`以外的所有字符。

对于码点大于0xFFFF的Unicode字符，点字符会认为是两个字符。

```text
/c.t/
```

以上的正则会匹配c和t之间包含一个任意字符的情况，如`cat`,`cit`。

#### 位置字符： 字符串开始\(^\)和字符串结束\($\)

```text
/^test/会匹配"test123"
/test$/会匹配"123test"
/^test$/匹配从开始位置到结束位置只有test:"test"
```

#### 选择符\(\|\)

选择符在正则中表示"or",`cat|dog`匹配cat或者dog

```text
/11|22/将会匹配"11"or"22"而不是匹配"112"or"122"
```

选择符可以连用，如：

```text
/aa|bb|cc/
```

### 转义符\(\)

转义符在正则表达式中匹配有特殊含义的元字符本身 需要加反斜杠转义的，一共有：`^`、`.`、`[`、`$`、`(`、`)`、`|`、`*`、`+`、`?`、`{`和`\\` 如果使用RegExp生成正则对象，需要使用两个斜杠，字符串内部会先转义一次，因为在字符串内部，反斜杠也是转义字符，所以会被反斜杠先转义一次。

### 特殊字符

* `\cX`表示`Ctrl-[X]`,其中的X是A-Z之间的任意一个英文字母，用来匹配控制字符
* `[\b]`匹配退格键\(U+0008\)，不要与`\b`混淆。
* `\n`匹配换行键
* `\r`匹配回车键
* `\t`匹配制表符tab\(U+0009\)
* `\v`匹配垂直制表符\(U+000B\)
* `\f`匹配换页符\(U+000C\)
* `\0`匹配null字符\(U+0000\)
* `\xhh`匹配一个以两位十六进制数\(`\x00-\xFF`\)表示的字符
* `\uhhhh`匹配一个以四位十六进制数\(`\u0000`-`\uFFFF`\)表示的Unicode字符。

  **字符类\(\[\]\)**

  字符类指可以选择一系列字符，只要匹配其中一个即可，所有供选择的字符都放在方括号内\[\]，如`[xyz]`表示`x`,`y`,`z`之中任选一个匹配。

`/[abc]/`可以匹配"app" `/[abc]/`不可以匹配"hello"

#### 脱字符\(^\)

如果方括号内部的第一个字符是`^`，则表示除了字符类之中的字符，匹配其他的字符，如`[^abc]`匹配除了a,b,c之外的字符。 如果方括号里没有其他字符，即只有`[^]`，就表示匹配一切字符，包括换行符。\(`.`\)不包括换行符。 **脱字符只有在字符类的第一个位置才有特殊含义，否则就是字面含义。**

#### 连字符\(-\)

某些情况下，对于连续序列，连字符\(`-`\)可以提供简写形式，表示字符的连续范围。比如，`[abcd]`可以写成`[a-d]`,`[0123456789]`可以写成`[0-9]`，`[A-Z]`表示26个大写字母。 只有连字符在方括号中才具备简写作用。

`[A-z]`不是表示大写字母和小写字母，在ASCII编码中，大写字母和小写字母之间还有其他的字符，会导致意外。

### 预定义模式

预定义模式指的是某些常见模式的简写方式 `\d` 匹配0-9之间的任一数字，相当于`[0-9]`。 `\D` 匹配所有0-9以外的字符，相当于`[^0-9]`。 `\w`匹配任意的字母、数字和下划线，相当于`[A-Za-z0-9_]`。 `\W` 除所有字母、数字和下划线以外的字符，相当于`[^A-Za-z0-9_]`。 `\s`匹配空格（包括换行符、制表符、空格符等），相等于`[ \t\r\n\v\f]`。 `\S` 匹配非空格的字符，相当于`[^ \t\r\n\v\f]`。 `\b`匹配词的边界。 `\B` 匹配非词边界，即在词的内部。

通常，正则表达式遇到换行符\(`\n`\)就会停止匹配。 `[\S\s]`可以指代一切字符

### 重复类\({}\)

模式的精确匹配次数，使用\(`{}`\)表示。`{n}`表示恰好重复n次，`{n,}`表示至少重复n次，`{n,m}`表示重复不少于n次，不多于m次

### 量词符

量词符用来设定某个模式出现的次数

* `?`问号表示某个模式出现0次或者1次，等同于`{0,1}`
* `*`星号表示某个模式出现0次或者多次，等同于`{0,}`
* `+`加号表示某个模式出现1次或多次，等同于`{1,}`

  **贪婪模式**

  量词在默认情况下都是最大可能匹配，即匹配直到下一个字符不满足匹配规则为止，这被称为贪婪模式。

```text
/a+/是贪婪模式，会匹配到a不出现为止
/a+?/非贪婪模式，条件一旦满足，就不再进行匹配。
```

除了非贪婪模式的加号，还有非贪婪模式的星号\(`*`\)

* `*?`表示某个模式出现0次或者多次，匹配时采用非贪婪模式
* `+?`表示某个模式出现1次或者多次，匹配时采用非贪婪模式

### 修饰符

修饰符表示模式的附加规则，放在正则模式的最尾部。 修饰符可以单个使用，也可以多个使用。

```text
//单个修饰符
var regex=/test/i;
//多个修饰符
var regex=/test/ig;
```

#### g修饰符

默认在一次匹配成功后，正则对象会停止向下匹配。g表示全局匹配\(global\)。加上它以后，正则对象将匹配全部符合条件的结果，主要用于搜索和替换。

正则不含g修饰符，每次都是从字符串头部匹配。 正则含g修饰符，每次都是从上一次匹配成功处开始向后匹配。

#### i修饰符

默认情况下，正则对象区分字母的大小写，加上i修饰符以后表示忽略大小写（ignorecase）。

#### m 修饰符

m修饰符表示多行模式（multiline），会修改^和$的行为。 默认情况下（即不加m修饰符时），`^`和`$`匹配字符串的开始处和结尾处，加上m修饰符以后，`^`和`$`还会匹配行首和行尾，即`^`和`$`会识别换行符`（\n）`。

```text
/world$/.test('hello world\n') // false
/world$/m.test('hello world\n') // true
```

### 组匹配

#### 括号表示分组匹配

正则表达式的括号表示分组匹配，括号中的模式可以用来匹配分组的内容。 注意，使用组匹配时，不宜同时使用g修饰符，否则match方法不会捕获分组的内容。

使用正则表达式的exec方法，配合循环，才能读到每一轮匹配的组捕获。

```text
var str = 'abcabc';
var reg = /(.)b(.)/g;
while (true) {
  var result = reg.exec(str);
  if (!result) break;
  console.log(result);
}
// ["abc", "a", "c"]
// ["abc", "a", "c"]
```

正则表达式内部，还可以用\n引用括号匹配的内容，n是从1开始的自然数，表示对应顺序的括号。

```text
/(.)b(.)\1b\2/.test("abcabc")
// true
```

`\1`表示第一个括号匹配的内容，`\2`表示第二个括号匹配的内容

括号还可以嵌套。

```text
/y((..)\2)\1/.test('yabababab') // true
```

`\1`指向外层括号，`\2`指向内层括号。

#### 非捕获组

\(?:x\)称为非捕获组（Non-capturing group），表示不返回该组匹配的内容，即匹配的结果中不计入这个括号。

```text
var m = 'abc'.match(/(?:.)b(.)/);
m // ["abc", "c"]
```

第一个括号是非捕获组，所以最后返回的结果中没有第一个括号，只有第二个括号匹配的内容。

#### 先行断言

x\(?=y\)称为先行断言（Positive look-ahead），x只有在y前面才匹配，y不会被计入返回结果。

“先行断言”中，括号里的部分是不会返回的。

```text
var m = 'abc'.match(/b(?=c)/);
m // ["b"]
```

b在c前面所以被匹配，但是括号对应的c不会被返回。

#### 先行否定断言

x\(?!y\)称为先行否定断言（Negative look-ahead），x只有不在y前面才匹配，y不会被计入返回结果。

```text
/\d+(?!\.)/.exec('3.14')
// ["14"]
```

只有不在小数点前面的数字才会被匹配，因此返回的结果就是14。

“先行否定断言”中，括号里的部分是不会返回的。

## RegExp对象

### 概述

新建正则表达式有两种方法。一种是使用字面量，以斜杠表示开始和结束。

```text
var regex = /xyz/;
```

一种是使用RegExp构造函数。 RegExp构造函数还可以接受第二个参数，表示修饰符\(i,g,m\);

```text
var regex = new RegExp('xyz');
```

第一种在引擎编译代码时新建正则表达式； 第二种方法在运行时新建正则表达式， **第一种效率高**

### 实例属性

一类是修饰符相关，返回一个布尔值，表示对应的修饰符是否设置，属性为**只读**。

* RegExp.prototype.ignoreCase：返回一个布尔值，表示是否设置了i修饰符。
* RegExp.prototype.global：返回一个布尔值，表示是否设置了g修饰符。
* RegExp.prototype.multiline：返回一个布尔值，表示是否设置了m修饰符。

另一类是与修饰符无关的属性

* RegExp.prototype.lastIndex：返回一个数值，表示下一次开始搜索的位置。该属性可读写，但是只在进行连续搜索时有意义，详细介绍请看后文。
* RegExp.prototype.source：返回正则表达式的字符串形式（不包括反斜杠），该属性只读。

### 实例方法

#### RegExp.prototype.test\(\)

返回一个布尔值，表示当前模式是否能匹配参数字符串。 如果正则表达式带有g修饰符，则每一次test方法都从上一次结束的位置开始向后匹配。 如果正则模式是一个空字符串，则匹配所有字符串。

#### RegExp.prototype.exec\(\)

用来返回匹配结果。如果发现匹配，就返回一个数组，成员是匹配成功的子字符串，否则返回null。

exec方法的返回数组还包含以下两个属性：

* input：整个原字符串。
* index：整个模式匹配成功的开始位置（从0开始计数）。

如果正则表达式加上g修饰符，则可以使用多次exec方法，下一次搜索的位置从上一次匹配成功结束的位置开始。

### 字符串的实例方法

#### String.prototype.match\(\)：

对字符串进行正则匹配，返回一个数组，成员是所有匹配的子字符串。如果正则表达式带有g修饰符，则该方法与正则对象的exec方法行为不同，会一次性返回所有匹配成功的结果。match方法无效，匹配总是从字符串的第一个字符开始。

#### String.prototype.search\(\)：

返回第一个满足条件的匹配结果在整个字符串中的位置。如果没有任何匹配，则返回-1。

#### String.prototype.replace\(\)：

替换匹配的值。它接受两个参数，第一个是正则表达式，表示搜索模式，第二个是替换的内容。返回替换后的字符串。正则表达式如果不加g修饰符，就替换第一个匹配成功的值，否则替换所有匹配成功的值。replace方法的第二个参数可以使用美元符号$，用来指代所替换的内容。

* $&：匹配的子字符串。
* $\`：匹配结果前面的文本。
* $’：匹配结果后面的文本。
* $n：匹配成功的第n组内容，n是从1开始的自然数。
* $$：指代美元符号$。

replace方法的第二个参数还可以是一个函数，将每一个匹配内容替换为函数返回值。

作为replace方法第二个参数的替换函数，可以接受多个参数。其中，第一个参数是捕捉到的内容，第二个参数是捕捉到的组匹配（有多少个组匹配，就有多少个对应的参数）。此外，最后还可以添加两个参数，倒数第二个参数是捕捉到的内容在整个字符串中的位置（比如从第五个位置开始），最后一个参数是原字符串。

#### String.prototype.split\(\)

按照给定规则进行字符串分割，返回一个数组，包含分割后的各个成员。

```text
str.split(separator, [limit])
```

该方法接受两个参数，第一个参数是正则表达式，表示分隔规则，第二个参数是返回数组的最大成员数。

如果正则表达式带有括号，则括号匹配的部分也会作为数组成员返回。

```text
'aaa*a*'.split(/(a*)/)
// [ '', 'aaa', '*', 'a', '*' ]
```

上面代码的正则表达式使用了括号，第一个组匹配是aaa，第二个组匹配是a，它们都作为数组成员返回。

