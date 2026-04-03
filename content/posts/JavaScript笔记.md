+++
date = 2026-03-21T21:37:35+08:00
title = "JavaScript笔记"
description = "JavaScript笔记"
slug = "javascript-note"
tags = ["ProgrammingLanguage","Software"]
+++

# JavaScript笔记

[现代JavaScript教程](https://zh.javascript.info/)

## 1. 简介

### JavaScript简介

浏览器中js的限制：不能读写硬盘文件，同源策略，标签页隔离，申请权限需要用户的许可

js上层语言：CoffeeScript， TypeScript/Flow，Dart， Kotlin， Brython

### 手册与规范

[ECMA262](https://tc39.es/ecma262/)， [MDN WebDocs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)， [caniuse](https://caniuse.com/)

### 代码编辑器

IDE：VSCode，WebStorm

轻量编辑器：SublimeText， notepad++

### 开发者控制台

Chrome开发者工具（F12）

Safari需要先在设置中打开开发模式

## 2.  JavaScript基础知识

### HelloWorld

在html中

```html
<!DOCTYPE HTML>
<html>
<body>
  <p>script 标签之前...</p>
  <script>
    alert('Hello, world!')
    console.log('Hello, world!')
  </script>
  <p>...script 标签之后</p>
</body>
</html>
```

**`script`标签的属性（attribute）：**

- `type` 特性：`<script type=…>`

  在老的 HTML4 标准中，要求 script 标签有 `type` 特性。通常是 `type="text/javascript"`。这样的特性声明现在已经不再需要。而且，现代 HTML 标准已经完全改变了此特性的含义。现在，它可以用于 JavaScript 模块。

- `language` 特性：`<script language=…>`

  这个特性是为了显示脚本使用的语言。这个特性现在已经没有任何意义，因为语言默认就是 JavaScript。不再需要使用它了。

`script`标签中的html注释：为了在不支持js的浏览器中隐藏js代码

外部脚本：src属性

```html
<script src="/path/to/script.js"></script>
```

> 注意：独立文件的好处是浏览器会下载它，然后将它保存到浏览器的 [缓存](https://en.wikipedia.org/wiki/Web_cache) 中。
>
> 如果设置了 `src` 特性，`script` 标签内容将会被忽略。

### 代码结构

语句：语句是执行行为（action）的语法结构和命令，语句之间可以使用分号进行分割。

分号：当存在换行符（line break）时，在大多数情况下可以省略分号。这也被称为 [自动分号插入](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion)。**分号省略有时会出现问题**

注释：//单行注释， /**/多行注释，同C/C++，java， go等

### 现代模式 "use strict"

`"use strict"`放在js文件开头或者函数体开头，可以全局/局部开启严格模式，一旦开启无法取消

现代 JavaScript 支持 “class” 和 “module” —— 高级语言结构，它们会自动启用 `use strict`。因此，如果我们使用它们，则无需添加 `"use strict"` 指令。

**不采用严格模式：**

可以不使用 `let` 进行变量声明，而可以简单地通过赋值来创建一个变量。严格模式下会报错

```js
num = 5
```

### 变量

```js
let a; a = 1
// 或
let a = 1
// 常量不能重新赋值
const b = 2

let user = 'John', age = 25, message = 'Hello'
// 老式的写法，会导致一些问题（变量声明提升，无块级作用域等）
var message = 'Hello'
```

**变量命名：**

1. 只能由数字字母下划线美元符号组成（同java）（可以使用非英文字母，但不推荐）
2. 不能以数字开头
3. 不能与关键字重名
4. 严格区分大小写
5. 一般采用小驼峰命名

**常量命名：**

- 全大写+下划线：仅用作“硬编码（hard-coded）”值的别名。如apikey，baseURL等
- 小驼峰：执行期间被“计算”出来，但初始赋值之后就不会改变。

**变量应当以一种容易理解变量内部是什么的方式进行命名。**

### 数据类型

有 8 种基本的数据类型（7 种原始类型和 1 种引用类型）

| 数据类型  | 特殊数值或说明                                               |
| --------- | ------------------------------------------------------------ |
| Number    | `Infinity`、`-Infinity` 和 `NaN`                             |
| BigInt    | 可以通过将 `n` 附加到整数字段的末尾来创建 `BigInt` 值        |
| String    | 单引号双引号反引号都可；反引号是**功能扩展**引号，允许我们通过将变量和表达式包装在 `${…}` 中，来将它们嵌入到字符串中 |
| Boolean   | 仅包含两个值：`true` 和 `false`                              |
| null      | 只包含 `null` 值,仅仅是一个代表“无”、“空”或“值未知”的特殊值。 |
| undefined | 只包含 `undefined` 值,含义是 `未被赋值`。                    |
| Symbol    | `symbol` 类型用于创建对象的唯一标识符                        |
| Object    | 唯一的引用类型，用于储存数据集合和更复杂的实体。             |

`NaN` 代表一个计算错误。它是一个不正确的或者一个未定义的数学操作所得到的结果；`NaN` 是粘性的。任何对 `NaN` 的进一步数学运算都会返回 `NaN`

在 JavaScript 中做数学运算是安全的。在 JavaScript 中做数学运算是安全的。

**typeof 运算符：**返回string类型的数据类型信息

- `typeof null == 'object'` `typeof` 的行为在这里是错误的。

- `typeof(x)`。它与 `typeof x` 相同。

### 交互：alert、prompt 和 confirm

```js
alert("Hello") // 返回undefined

result = prompt(title, [defaultPlaceHolder]) //如果我们不提供的话，Internet Explorer 会把 "undefined" 插入到 prompt

result = confirm(question); //点击确定返回 true，点击取消返回 false
```

这些方法都是模态的：它们暂停脚本的执行，并且不允许用户与该页面的其余部分进行交互，直到窗口被解除；无法修改外观

### 类型转换

这里只讨论原始类型的转换

`String()`：其他类型转换为字符串

`Number()`：

| 值              | 变成……                                                       |
| :-------------- | :----------------------------------------------------------- |
| `undefined`     | `NaN`                                                        |
| `null`          | `0`                                                          |
| `true 和 false` | `1` and `0`                                                  |
| `string`        | 去掉首尾空白字符（空格、换行符 `\n`、制表符 `\t` 等）后的纯数字字符串中含有的数字。如果剩余字符串为空，则转换结果为 `0`。否则，将会从剩余字符串中“读取”数字。当类型转换出现 error 时返回 `NaN`（也就是说只能转换数字开头的字符串） |

`Boolean()`：

- 直观上为“空”的值（如 `0`、空字符串、`" "`、`null`、`undefined` 和 `NaN`）将变为 `false`。
- 其他值变成 `true`。

**注意：包含 0 的字符串** `"0"` **是** `true`

### 基础运算符、数学运算

**一元运算符、二元运算符、运算元**

**数学运算：**+ - * / % **

“+”可以连接字符串：任意一个运算元是字符串，那么另一个运算元也将被转化为字符串

“+”可以将其他类型转换为数字进行运算

**运算符优先级：**一元运算符优先级最高[优先级表](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

**赋值运算符：**优先级为2，仅高于 ，运算符，语句 `x = value` 将值 `value` 写入 `x` **然后返回 value**。

**链式赋值：** `a = b = c = 2 + 2`

**原地修改：**增强赋值运算符

**自增自减运算符：**a++和++a的区别同C（这里有返回值，不同于go语言）

**位运算符：**& | ^ ~ << >> >>>很少用（密码学库可能会用到）

**逗号运算符：**能让我们处理多个表达式，使用 `,` 将它们分开。每个表达式都运行了，但是只有最后一个的结果会被返回。

### 值的比较

`== <= >= != !== ===` 

所有比较运算符均返回布尔值

字符串是按字符（母）逐个进行比较的(按照Unicode编码顺序)

当对不同类型的值进行比较时，JavaScript 会首先将其转化为数字（number）再判定大小。

严格相等运算符 `===` 在进行比较时不会做任何的类型转换。

`null==undefined`

== 和 >=这些运算符逻辑独立，所以 null == 0为false（只等于undefined） null >= 0为true（null转换为数字0再比较）

`undefined` 不应该被与其他值进行比较（因为undefined会被转换为NaN，NaN与任何值比较都会返回false）

### 条件分支：if和？

```js
if (计算圆括号内的表达式，并将计算结果转换为布尔型){} //语句只有一行时大括号可以省略

if (){
    
}else if{
    
}
if (){
    
}else{
    
}
// 三元运算符
let result = condition ? value1 : value2
```

### 逻辑运算符

&& ||均有短路特性

```js
result = value1 || value2 || value3 //返回从左到右的第一个“真值”，如果没有真值，返回最后一个操作数

true || alert("not printed");
false || alert("printed"); // 短路求值

result = value1 && value2 && value3 // 返回从左到右的第一个“假值”，如果没有假值，返回最后一个操作数
// 与运算 && 的优先级比或运算 || 要高。
result = !value //将操作数转化为布尔类型：true/false；返回相反的值
```

### 空值合并运算符 ??

当一个值既不是 `null` 也不是 `undefined` 时，我们将其称为“已定义的（defined）”。

`a ?? b` 的结果是：

- 如果 `a` 是已定义的，则结果为 `a`，
- 如果 `a` 不是已定义的，则结果为 `b`。

??运算符的优先级与||相同，它们的优先级都为3

出于安全原因，JavaScript 禁止将 `??` 运算符与 `&&` 和 `||` 运算符一起使用，除非使用括号明确指定了优先级。

### 循环：while和for

```js
while (condition) {
  // 在 while 中的循环条件会被计算，计算结果会被转化为布尔值。
  // 所谓的“循环体”
  // 单行循环体不需要大括号
}

do {
  // 循环体
} while (condition)
    
for (begin; condition; step) {
  // ……循环体……
}

//这里“计数”变量 `i` 是在循环中声明的。这叫做“内联”变量声明。这样的变量只在循环中可见。
// `for` 循环的任何语句段都可以被省略。
```

break和continue: 禁止 `break/continue` 在 ‘?’(三元运算符) 的右边

标签：labelName：for(){}

### switch语句

```js
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
    [break]

  case 'value2':  // if (x === 'value2')
    ...
    [break]

  default:
    ...
    [break]
}
```

这里不会默认break，必须手动添加（不同于go语言）

共享同一段代码的几个 `case` 分支可以被分为一组

这里的相等是严格相等。被比较的值必须是相同的类型才能进行匹配。

### 函数

```js
// 函数声明
function name(parameter1, parameter2, ... parameterN) {
  ...body...
}
  
function showMessage(from, text = anotherFunction()) {
  // anotherFunction() 仅在没有给定 text 时执行
  // 其运行结果将成为 text 的值
}
```

局部变量：在函数中声明的变量只在该函数内部可见。

外部变量：函数对外部变量拥有全部的访问权限。函数也可以修改外部变量。

如果在函数内部声明了同名变量，那么函数会 **遮蔽** 外部变量。

- 参数（parameter）是函数声明中括号内列出的变量（它是函数声明时的术语）。
- 参数（argument）是调用函数时传递给函数的值（它是函数调用时的术语）。

空值的 `return` 或没有 `return` 的函数返回值为 `undefined`

不要在 `return` 与返回值之间添加新行（请使用括号）

### 函数表达式

```js
let sayHi = function() {
  alert( "Hello" )
}
```

回调函数：主要思想是我们传递一个函数，并期望在稍后必要时将其“回调”。

**函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用。**

**在函数声明被定义之前，它就可以被调用。**

**严格模式下，当一个函数声明在一个代码块内时，它在该代码块内的任何位置都是可见的。但在代码块外不可见。**

### 箭头函数，基础知识

```js
let func = (arg1, arg2, ..., argN) => expression
```

如果只有一个参数，还可以省略掉参数外的圆括号，使代码更短

如果函数体只有一行，那么可以省略大括号（此时会自动return 表达式的结果）

### JavaScript特性

- 代码用分号分割（或自动分号插入）

- 严格模式 `"use strict"`

- 变量：let const var
- 8种数据类型
- 交互：prompt alert confirm
- 运算符：数学运算，位运算，逻辑运算，空值合并，比较运算，赋值，三元运算，逗号运算

- 分支if switch与循环while dowhile for

- 函数：函数声明，函数表达式

## 3. 代码质量

### 在浏览器中调试

可以使用 `debugger` 命令来暂停代码（只有在开发者工具打开时才有效，否则浏览器会忽略它）

日志记录：`console.log()`

### 代码风格

花括号：行尾风格

行的长度：通常是 80 或 120 个字符。

缩进：4个空格；不应该出现连续超过 9 行都没有被垂直分割的代码。

分号：最好不要省略

嵌套层级：不要过深

函数位置：先写调用代码，再写函数定义

风格指南：团队一致

自动检查器：JSLint、ESLint、JSHint

### 注释

少写解释性注释

**描述架构**

**记录函数的参数和用法**

**重要的解决方案，特别是在不是很明显时**

### 忍者代码

> 检测到讽刺意味！
>
> ​    许多人试图追随忍者的脚步。只有极少数成功了。

### 使用Mocha进行自动化测试

行为驱动开发：BDD 包含了三部分内容：测试、文档和示例

规范有三种使用方式：

1. 作为 **测试** —— 保证代码正确工作。
2. 作为 **文档** —— `describe` 和 `it` 的标题告诉我们函数做了什么。
3. 作为 **案例** —— 测试实际工作的例子展示了一个函数可以被怎样使用。

### Polyfill和转译器

[转译器](https://en.wikipedia.org/wiki/Source-to-source_compiler) 是一种可以将源码转译成另一种源码的特殊的软件。它可以解析（“阅读和理解”）现代代码，并使用旧的语法结构对其进行重写，进而使其也可以在旧的引擎中工作。

说到名字，[Babel](https://babeljs.io/) 是最著名的转译器之一。

现代项目构建系统，例如 [webpack](https://webpack.js.org/)，提供了在每次代码更改时自动运行转译器的方法，因此很容易将代码转译集成到开发过程中。

更新/添加新函数的脚本被称为“polyfill”。它“填补”了空白并添加了缺失的实现。

## 4. Object（对象）：基础知识

### 对象

```js
let user = new Object(); // “构造函数” 的语法
let user = {};  // “字面量” 的语法

let user = {     // 一个对象
  name: "John",  // 键 "name"，值 "John"
  age: 30,        // 键 "age"，值 30
  "likes birds": true,
};

// 读取文件的属性：
alert( user.name ); // John
alert( user.age ); // 30

user.isAdmin = true;

delete user.age;
// 读取
alert(user["likes birds"]); // 方括号里可以写变量

// 计算属性
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
  [fruit]: 5, // 属性名是从 fruit 变量中得到的
};
```

属性名跟变量名一样。可以使用 **属性值缩写** 方法，使属性名变得更短。可以用 `name` 来代替 `name:name` 

属性命名没有限制。属性名可以是任何字符串或者 symbol（一种特殊的标志符类型，将在后面介绍）。其他类型会被自动地转换为字符串。

一个名为 `__proto__` 的属性。我们不能将它设置为一个非对象的值：

能够被访问任何属性。**即使属性不存在也不会报错！**读取不存在的属性会得到 `undefined`

in操作符：`"key" in object`

```js
for (key in object) {
  // 对此对象属性中的每个键执行的代码
}
```

对象属性的顺序：整数属性（一个可以在不做任何更改的情况下与一个整数进行相互转换的字符串）会被进行排序，其他属性则按照创建的顺序显示。

### 对象引用与复制

类似 `obj1 > obj2` 的比较，或者跟一个原始类型值的比较 `obj == 5`，对象都会被转换为原始值。

克隆：可以创建一个新对象，通过遍历已有对象的属性，并在原始类型值的层面复制它们，以实现对已有对象结构的复制；或者：

```js
Object.assign(dest, [src1, src2, src3...]) //该方法将所有源对象的属性拷贝到目标对象 dest 中（浅拷贝）
```

如果被拷贝的属性的属性名已经存在，那么它会被覆盖

或者使用spread语法

**深层克隆**：采用现有的实现，例如 [lodash](https://lodash.com/) 库的 [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep)。

### 垃圾回收

- 垃圾回收是自动完成的，我们不能强制执行或是阻止执行。
- 当对象是可达状态时，它一定是存在于内存中的。
- 被引用与可访问（从一个根）不同：一组相互连接的对象可能整体都不可达，正如我们在上面的例子中看到的那样。

### 对象方法，this

作为对象属性的函数被称为 **方法**，为了访问该对象，方法中可以使用 `this` 关键字，谁调用的该方法，`this`就是谁（.前面的对象）

`this` 的值是在代码运行时计算出来的，它取决于代码上下文。

在没有对象的情况下调用：`this == undefined`（严格模式下，非严格模式下`this`是 `global` 或者 `window` ）

箭头函数有些特别：它们没有自己的 `this`。如果我们在这样的函数中引用 `this`，`this` 值取决于外部“正常的”函数。

### 构造器和操作符“new”

构造函数：它们的命名以大写字母开头；它们只能由 `"new"` 操作符来执行。

```js
function User(name) {
    this.name = name
    this.isAdmin = false
    this.sayHi = function() {
    	alert( "My name is: " + this.name )
    }
}
let user = new User("Jack")
```

**new.target**:在一个函数内部，我们可以使用 `new.target` 属性来检查它是否被使用 `new` 进行调用了。对于常规调用，它为 undefined，对于使用 `new` 的调用，则等于该函数

**构造函数的return**：如果return一个对象，那么返回该对象而不是this，如果return原始值，则忽略（返回this）

> 如果构造函数无参数，则括号可以省略，但不推荐

### 可选链“?.”

如果可选链 `?.` 前面的值为 `undefined` 或者 `null`，它会停止运算并返回 `undefined`。

`?.()` 用于调用一个可能不存在的函数。`userAdmin.admin?.()`如果admin是函数则调用，否则忽略

`?.[]` 允许从一个可能不存在的对象上安全地读取属性。

### symbol类型

只有两种原始类型可以用作对象属性键：字符串类型、symbol 类型

```js
let id = Symbol()
let id = Symbol("id") // 描述可选
alert(id.toString())  // Symbol(id)
alert(id.description) // id
```

symbol 允许我们创建对象的“隐藏”属性（使用symbol作为key）

symbol 属性不参与 `for..in` 循环（`Object.keys()`会忽略他们，但`Object.assign()`会复制它们）

全局symbol：注册或读取：`Symbol.for("id")`，通过symbol查找：`Symbol.keyFor(sym)`

系统Symbol：一些内置方法挂在了Symbol的原型对象上

symbol 不是 100% 隐藏的。有一个内建方法 [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) 允许我们获取所有的 symbol。还有一个名为 [Reflect.ownKeys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) 的方法可以返回一个对象的 **所有** 键，包括 symbol。

### 对象——原始值转换

转换为boolean：始终为true

转换为number：对象相减或应用数学函数时

转换为string：在需要字符串的场合

三种hint：string，number，default

除了一种情况（`Date` 对象）之外，所有内建对象都以和 `"number"` 相同的方式实现 `"default"` 转换。我们也可以这样做。

1. 调用 `obj[Symbol.toPrimitive](hint)` —— 带有 symbol 键 `Symbol.toPrimitive`（系统 symbol）的方法，如果这个方法存在的话，
2. 否则，如果 hint 是 `"string"` —— 尝试调用 `obj.toString()` 或 `obj.valueOf()`，无论哪个存在。
3. 否则，如果 hint 是 `"number"` 或 `"default"` —— 尝试调用 `obj.valueOf()` 或 `obj.toString()`，无论哪个存在。

> 一句话：优先调用`Symbol.toPrimitive()`，剩下按类型优先调用`toString`或者`valueOf`

这些转换可以返回任意原始类型

## 5. 数据类型

### 原始类型的方法

在访问原始类型的方法时，会创建对象包装器在调用方法，调用完后这个对象会被销毁

null和undefined没有任何方法

### 数字类型

可以使用_作为分隔符

可以使用e表示科学计数法

可以使用0x 0b 0o表示不同进制数字

`num.toString(base)`将数字zhuanhuanwbase进制的字符串（不含0x等前导标志）

Math.floor()

Math.ceil()

Math.round()

Math.trunc()

以上方法都返回整数

函数 [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) 将数字舍入到小数点后 `n` 位，并以字符串形式返回结果

`isNaN(value)` 将其参数转换为数字，然后测试它是否为 `NaN`

`isFinite(value)` 将其参数转换为数字，如果是常规数字而不是 `NaN/Infinity/-Infinity`，则返回 `true`

`Object.is`，它类似于 `===` 一样对值进行比较，但它对于两种边缘情况更可靠：

1. 它适用于 `NaN`：`Object.is(NaN, NaN) === true`，这是件好事。
2. 值 `0` 和 `-0` 是不同的：`Object.is(0, -0) === false`，从技术上讲这是对的，因为在内部，数字的符号位可能会不同，即使其他所有位均为零。

使用 `parseInt/parseFloat` 进行“软”转换，它从字符串中读取数字，然后返回在发生 error 前可以读取到的值。

Math.random() Math.max() Math.min() Math.pow()

### 字符串

单引号双引号反引号

tagged templates：`func`string。函数 `func` 被自动调用，接收字符串和嵌入式表达式 [docs](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals) 。

转义字符

`length` 属性表示字符串长度

**下标访问：**`[pos]`（找不到返回undefined）或者`str.charAt(pos)`（找不到返回空字符串）

可以使用for of遍历，字符串是原始类型，是不可变的

[toLowerCase()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) 和 [toUpperCase()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) 方法可以改变大小写

**查找子串：**

[str.indexOf(substr, pos)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)它从给定位置 `pos` 开始，在 `str` 中查找 `substr`，如果没有找到，则返回 `-1`，否则返回匹配成功的位置。[str.lastIndexOf(substr, pos)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf)

对于 32-bit 整数，`~n` 等于 `-(n+1)`

[str.includes(substr, pos)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/includes) 根据 `str` 中是否包含 `substr` 来返回 `true/false`

方法 [str.startsWith](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith) 和 [str.endsWith](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith) 

**获取子串：**

`str.slice(start [, end])`返回字符串从 `start` 到（但不包括）`end` 的部分。

`str.substring(start [, end])`返回字符串从 `start` 到（但不包括）`end` 的部分。这与 `slice` 几乎相同，但它允许 `start` 大于 `end`。

`str.substr(start [, length])`返回字符串从 `start` 开始的给定 `length` 的部分。

**比较字符串：**

`str.codePointAt(pos)`返回在 `pos` 位置的字符代码 :

`String.fromCodePoint(code)`通过数字 `code` 创建字符

[str.localeCompare(str2)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare) 会根据语言规则返回一个整数，这个整数能指示字符串 `str` 在排序顺序中排在字符串 `str2` 前面、后面、还是相同：

- 如果 `str` 排在 `str2` 前面，则返回负数。
- 如果 `str` 排在 `str2` 后面，则返回正数。
- 如果它们在相同位置，则返回 `0`。

`str.trim()` —— 删除字符串前后的空格 (“trims”)。

`str.repeat(n)` —— 重复字符串 `n` 次。

代理对和unicode转义

### 数组

数组是特殊的对象，是引用类型

```js
let arr = new Array();
let arr = [];
```

换句话说，`arr.at(i)`：如果 `i >= 0`，则与 `arr[i]` 完全相同；对于 `i` 为负数的情况，它则从数组的尾部向前数。

- `push` 在末端添加一个或多个元素.
- `pop` 从末端取出一个元素.
- `shift` 取出队列首端的一个元素，整个队列往前移，这样原先排第二的元素现在排在了第一。

- `unshift`在数组的首端添加一个或多个元素

`push/pop` 方法运行的比较快，而 `shift/unshift` 比较慢（需要依次移动元素）

`for..of` 不能获取当前元素的索引，只是获取元素值

`for..in` 循环会遍历 **所有属性**，不仅仅是这些数字属性，所以不要使用

`length` 属性会自动更新。准确来说，它实际上不是数组里元素的个数，而是最大的数字索引值加一（length属性可读可写，手动减小length会截断数组）

数组有自己的 `toString` 方法的实现，会返回以逗号隔开的元素列表。

**数组方法：**

```js
arr.splice(start[, deleteCount, elem1, ..., elemN])
```

它从索引 `start` 开始修改 `arr`：删除 `deleteCount` 个元素并在当前位置插入 `elem1, ..., elemN`。最后返回被删除的元素所组成的数组。（允许负向索引）

```js
arr.slice([start], [end])
```

它会返回一个新数组，将所有从索引 `start` 到 `end`（不包括 `end`）的数组项复制到一个新的数组。`start` 和 `end` 都可以是负数，在这种情况下，从末尾计算索引。

```js
arr.concat(arg1, arg2...)
```

它接受任意数量的参数 —— 数组或值都可以。

结果是一个包含来自于 `arr`，然后是 `arg1`，`arg2` 的元素的新数组。

如果参数 `argN` 是一个数组，那么其中的所有元素都会被复制。否则，将复制参数本身。

```js
arr.forEach(function(item, index, array) {
    // 为每个元素运行一个函数
    //该函数的结果（如果它有返回）会被抛弃和忽略。
})
```

[arr.indexOf](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) 和 [arr.includes](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 方法语法相似，并且作用基本上也与字符串的方法相同，只不过这里是对数组元素而不是字符进行操作：

- `arr.indexOf(item, from)` —— 从索引 `from` 开始搜索 `item`，如果找到则返回索引，否则返回 `-1`。
- `arr.includes(item, from)` —— 从索引 `from` 开始搜索 `item`，如果找到则返回 `true`（译注：如果没找到，则返回 `false`）。（includes可以正确处理NaN）

```js
let result = arr.find(function(item, index, array) {
  // 如果返回 true，则返回 item 并停止迭代
  // 对于假值（falsy）的情况，则返回 undefined
})
```

[arr.findIndex](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) 方法（与 `arr.find`）具有相同的语法，但它返回找到的元素的索引，而不是元素本身。如果没找到，则返回 `-1`。

[arr.findLastIndex](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/findLastIndex) 方法类似于 `findIndex`，但从右向左搜索，类似于 `lastIndexOf`。

```js
let results = arr.filter(function(item, index, array) {
  // 如果 true item 被 push 到 results，迭代继续
  // 如果什么都没找到，则返回空数组
  // 返回的是所有匹配元素组成的数组
})
```

```js
let result = arr.map(function(item, index, array) {
  // 返回新值而不是当前元素
  // 它对数组的每个元素都调用函数，并返回结果数组
})
```

[arr.sort](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 方法对数组进行 **原位（in-place）** 排序，更改元素的顺序,它还返回排序后的数组(就是arr)**默认情况下被按字符串进行排序**可传入参数作为比较依据

```js
arr.sort((a, b) => a - b) //从小到大排序
```

[arr.reverse](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) 方法用于颠倒 `arr` 中元素的顺序。

[str.split(delim)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/String/split) 方法通过给定的分隔符 `delim` 将字符串分割成一个数组。第二个参数是数组长度限制，传入空字符串会拆分为字符

[arr.join(glue)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/join) 与 `split` 相反。它会在它们之间创建一串由 `glue` 粘合的 `arr` 项

```js
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);
```

[Array.isArray(value)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)。如果 `value` 是一个数组，则返回 `true`；否则返回 `false`

```js
arr.find(func, thisArg);
arr.filter(func, thisArg);
arr.map(func, thisArg);
// ...
// thisArg 是可选的最后一个参数，指定函数中的this指向
```

[arr.some(fn)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/some)/[arr.every(fn)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/every) 检查数组。

与 `map` 类似，对数组的每个元素调用函数 `fn`。如果任何/所有结果为 `true`，则返回 `true`，否则返回 `false`

[arr.fill(value, start, end)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) —— 从索引 `start` 到 `end`，用重复的 `value` 填充数组。

[arr.copyWithin(target, start, end)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin) —— 将从位置 `start` 到 `end` 的所有元素复制到 **自身** 的 `target` 位置（覆盖现有元素）。

[arr.flat(depth)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)/[arr.flatMap(fn)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) 从多维数组创建一个新的扁平数组。

[`Array.of(element0[, element1[, …[, elementN\]]])`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/of) 基于可变数量的参数创建一个新的 `Array` 实例，而不需要考虑参数的数量或类型。

### 可迭代对象

为了让对象可迭代（也就让 `for..of` 可以运行）我们需要为对象添加一个名为 `Symbol.iterator` 的方法（一个专门用于使对象可迭代的内建 symbol）。

1. 当 `for..of` 循环启动时，它会调用这个方法（如果没找到，就会报错）。这个方法必须返回一个 **迭代器（iterator）** —— 一个有 `next` 方法的对象。
2. 从此开始，`for..of` **仅适用于这个被返回的对象**。
3. 当 `for..of` 循环希望取得下一个数值，它就调用这个对象的 `next()` 方法。
4. `next()` 方法返回的结果的格式必须是 `{done: Boolean, value: any}`，当 `done=true` 时，表示循环结束，否则 `value` 是下一个值。

数组和字符串是使用最广泛的内建可迭代对象（支持代理对）

**可迭代和类数组：**

- Iterable 是实现了 `Symbol.iterator` 方法的对象。
- Array-like 是有索引和 `length` 属性的对象，所以它们看起来很像数组。

`Array.from(obj[, mapFn, thisArg])` 将可迭代对象或类数组对象 `obj` 转化为真正的数组 `Array`，然后我们就可以对它应用数组的方法。可选参数 `mapFn` 和 `thisArg` 允许我们将函数应用到每个元素。

### Map and Set (映射和集合)

**[Map](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Map) 是一个带键的数据项的集合，就像一个 `Object` 一样。 但是它们最大的差别是 `Map` 允许任何类型的键（key）。**

- `new Map()` —— 创建 map。
- `map.set(key, value)` —— 根据键存储值。
- `map.get(key)` —— 根据键来返回值，如果 `map` 中不存在对应的 `key`，则返回 `undefined`。
- `map.has(key)` —— 如果 `key` 存在则返回 `true`，否则返回 `false`。
- `map.delete(key)` —— 删除指定键的值。
- `map.clear()` —— 清空 map。
- `map.size` —— 返回当前元素个数。

每一次 `map.set` 调用都会返回 map 本身，所以我们可以进行“链式”调用

- `map.keys()` —— 遍历并返回一个包含所有键的可迭代对象，
- `map.values()` —— 遍历并返回一个包含所有值的可迭代对象，
- `map.entries()` —— 遍历并返回一个包含所有实体 `[key, value]` 的可迭代对象，`for..of` 在默认情况下使用的就是这个。

迭代的顺序与插入值的顺序相同。与普通的 `Object` 不同，`Map` 保留了此顺序。

```js
// 对每个键值对 (key, value) 运行 forEach 函数
recipeMap.forEach( (value, key, map) => {
  alert(`${key}: ${value}`); // cucumber: 500 etc
});
```

使用内建方法 [Object.entries(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)，该方法返回对象的键/值对数组，该数组格式完全按照 `Map` 所需的格式。就可以使用new关键字创建map了

`Object.fromEntries` 方法的作用是相反的：给定一个具有 `[key, value]` 键值对的数组，它会根据给定数组创建一个对象：

**`Set` 是一个特殊的类型集合 —— “值的集合”（没有键），它的每一个值只能出现一次。**

- `new Set(iterable)` —— 创建一个 `set`，如果提供了一个 `iterable` 对象（通常是数组），将会从数组里面复制值到 `set` 中。
- `set.add(value)` —— 添加一个值，返回 set 本身
- `set.delete(value)` —— 删除值，如果 `value` 在这个方法调用的时候存在则返回 `true` ，否则返回 `false`。
- `set.has(value)` —— 如果 `value` 在 set 中，返回 `true`，否则返回 `false`。
- `set.clear()` —— 清空 set。
- `set.size` —— 返回元素个数。

```js
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```

- `set.keys()` —— 遍历并返回一个包含所有值的可迭代对象，
- `set.values()` —— 与 `set.keys()` 作用相同，这是为了兼容 `Map`，
- `set.entries()` —— 遍历并返回一个包含所有的实体 `[value, value]` 的可迭代对象，它的存在也是为了兼容 `Map`。

WeakMap and WeakSet (弱映射和弱集合)

`WeakMap` 的键必须是对象，不能是原始值，如果我们在 weakMap 中使用一个对象作为键，并且没有其他对这个对象的引用 —— 该对象将会被从内存（和map）中自动清除。

- `weakMap.get(key)`
- `weakMap.set(key, value)`
- `weakMap.delete(key)`
- `weakMap.has(key)`

`WeakSet` 的表现类似：

- 与 `Set` 类似，但是我们只能向 `WeakSet` 添加对象（而不能是原始值）。
- 对象只有在其它某个（些）地方能被访问的时候，才能留在 `WeakSet` 中。
- 跟 `Set` 一样，`WeakSet` 支持 `add`，`has` 和 `delete` 方法，但不支持 `size` 和 `keys()`，并且不可迭代。

### Object.keys，values，entries

- [Object.keys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) —— 返回一个包含该对象所有的键的数组。
- [Object.values(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/values) —— 返回一个包含该对象所有的值的数组。
- [Object.entries(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) —— 返回一个包含该对象所有 [key, value] 键值对的数组。

会忽略symbol属性，可以利用这些函数转换对象

### 解构赋值

```js
// 我们有一个存放了名字和姓氏的数组
let arr = ["John", "Smith"]

// 解构赋值
// 设置 firstName = arr[0]
// 以及 surname = arr[1]
let [firstName, surname] = arr
// 可以通过添加额外的逗号来丢弃数组中不想要的元素
// 等号左侧使用任何“可以被赋值的”东西。

// 默认值
let [name = "Guest", surname = "Anonymous"] = ["Julius"]

let {var1: v1, var2: v2} = {var1:…, var2:…}

// 嵌套解构赋值
let options = {
  size: {
    width: 100,
    height: 200
  },
  items: ["Cake", "Donut"],
  extra: true
};

// 为了清晰起见，解构赋值语句被写成多行的形式
let {
  size: { // 把 size 赋值到这里
    width,
    height
  },
  items: [item1, item2], // 把 items 赋值到这里
  title = "Menu" // 在对象中不存在（使用默认值）
} = options;
```

`...rest` 的值是数组中剩下的元素组成的数组。

我们可以用一个对象来传递所有参数，而函数负责把这个对象解构成各个参数：

```js
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

function showMenu({
  title = "Untitled",
  width: w = 100,  // width goes to w
  height: h = 200, // height goes to h
  items: [item1, item2] // items first element goes to item1, second to item2
} = {}) {
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}
```

### 日期和时间

`new Date(milliseconds)`创建一个 `Date` 对象，其时间等于 1970 年 1 月 1 日 UTC+0 之后经过的毫秒数（1/1000 秒）。

`new Date()`不带参数 —— 创建一个表示当前日期和时间的 `Date` 对象：

`new Date(datestring)`如果只有一个参数，并且是字符串，那么它会被自动解析。该算法与 `Date.parse` 所使用的算法相同

`new Date(year, month, date, hours, minutes, seconds, ms)`

使用当前时区中的给定组件创建日期。只有前两个参数是必须的。`year` 应该是四位数。为了兼容性，也接受 2 位数，并将其视为 `19xx`，例如 `98` 与 `1998` 相同，但强烈建议始终使用 4 位数。`month` 计数从 `0`（一月）开始，到 `11`（十二月）结束。`date` 是当月的具体某一天，如果缺失，则为默认值 `1`。如果 `hours/minutes/seconds/ms` 缺失，则均为默认值 `0`。

- [getFullYear()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getFullYear)

  获取年份（4 位数）

- [getMonth()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth)

  获取月份，**从 0 到 11**。

- [getDate()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate)

  获取当月的具体日期，从 1 到 31，这个方法名称可能看起来有些令人疑惑。

- [getHours()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getHours)，[getMinutes()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getMinutes)，[getSeconds()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getSeconds)，[getMilliseconds()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getMilliseconds)

  获取相应的时间组件。

- [getDay()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)

  获取一周中的第几天，从 `0`（星期日）到 `6`（星期六）。第一天始终是星期日，在某些国家可能不是这样的习惯，但是这不能被改变。

**以上的所有方法返回的组件都是基于当地时区的。**

当然，也有与当地时区的 UTC 对应项，它们会返回基于 UTC+0 时区的日、月、年等：[getUTCFullYear()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCFullYear)，[getUTCMonth()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCMonth)，[getUTCDay()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCDay)。只需要在 `"get"` 之后插入 `"UTC"` 即可。

还有两个没有 UTC 变体的特殊方法：

- [getTime()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)

  返回日期的时间戳 —— 从 1970-1-1 00:00:00 UTC+0 开始到现在所经过的毫秒数。

- [getTimezoneOffset()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getTimezoneOffset)

  返回 UTC 与本地时区之间的时差，以分钟为单位：

下列方法可以设置日期/时间组件：

- [`setFullYear(year, [month\], [date])`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setFullYear)
- [`setMonth(month, [date\])`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setMonth)
- [`setDate(date)`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setDate)
- [`setHours(hour, [min\], [sec], [ms])`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setHours)
- [`setMinutes(min, [sec\], [ms])`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setMinutes)
- [`setSeconds(sec, [ms\])`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setSeconds)
- [`setMilliseconds(ms)`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setMilliseconds)
- [`setTime(milliseconds)`](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/setTime)（使用自 1970-01-01 00:00:00 UTC+0 以来的毫秒数来设置整个日期）

除了 `setTime()` 都有 UTC 变体，例如：`setUTCHours()`。

**自动校准** 是 `Date` 对象的一个非常方便的特性。我们可以设置超范围的数值，它会自动校准。

如果我们仅仅想要测量时间间隔，我们不需要 `Date` 对象。有一个特殊的方法 `Date.now()`，它会返回当前的时间戳。

**[Date.parse(str)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/parse) 方法可以从一个字符串中读取日期。**

字符串的格式应该为：`YYYY-MM-DDTHH:mm:ss.sssZ`，其中：

- `YYYY-MM-DD` —— 日期：年-月-日。
- 字符 `"T"` 是一个分隔符。
- `HH:mm:ss.sss` —— 时间：小时，分钟，秒，毫秒。
- 可选字符 `'Z'` 为 `+-hh:mm` 格式的时区。单个字符 `Z` 代表 UTC+0 时区。

简短形式也是可以的，比如 `YYYY-MM-DD` 或 `YYYY-MM`，甚至可以是 `YYYY`。

`Date.parse(str)` 调用会解析给定格式的字符串，并返回时间戳（自 1970-01-01 00:00:00 起所经过的毫秒数）。如果给定字符串的格式不正确，则返回 `NaN`。

### JSON方法，toJSON

**方法 `JSON.stringify(student)` 接收对象并将其转换为字符串。**

一些特定于 JavaScript 的对象属性会被 `JSON.stringify` 跳过。

- 函数属性（方法）。
- Symbol 类型的键和值。
- 存储 `undefined` 的属性。

`JSON.stringify` 的完整语法是：

```javascript
let json = JSON.stringify(value[, replacer, space])
```

- value要编码的值。
- replacer要编码的属性数组或映射函数 `function(key, value)`。
- space用于格式化的空格数量。

对象也可以提供 `toJSON` 方法来进行 JSON 转换。如果可用，`JSON.stringify` 会自动调用它。

**要解码 JSON 字符串，我们需要另一个方法 [JSON.parse](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)。**

```javascript
let value = JSON.parse(str, [reviver]);
```

- str要解析的 JSON 字符串。
- reviver可选的函数 function(key,value)，该函数将为每个 `(key, value)` 对调用，并可以对值进行转换。

## 6. 函数进阶内容

### 递归和堆栈

函数会调用 **自身**。这就是所谓的 **递归**。

递归调用会使得函数执行上下文入栈或者出栈

### Rest 参数与 Spread 语法

可以在函数定义中声明一个数组来收集参数。语法是这样的：`...变量名`，这将会声明一个数组并指定其名称，其中存有剩余的参数。

**Rest 参数必须放到参数列表的末尾**

有一个名为 `arguments` 的特殊类数组对象可以在函数中被访问，该对象以参数在参数列表中的索引作为键，存储所有参数。箭头函数没有 `"arguments"`

当在函数调用中使用 `...arr` 时，它会把可迭代对象 `arr` “展开”到参数列表中。

- `Array.from` 适用于类数组对象也适用于可迭代对象。
- Spread 语法只适用于可迭代对象。

### 变量作用域，闭包

声明代码块会创建新的作用域，对于if、while和for也是如此

嵌套函数:词法环境：每个函数在创建时都会记住其词法环境

词法环境对象由两部分组成：

1. 环境记录（Environment Record） —— 一个存储所有局部变量作为其属性（包括一些其他信息，例如 this 的值）的对象。

2. 对 外部词法环境 的引用，与外部代码相关联。

闭包：嵌套函数，外层函数返回内层函数，内层函数引用外层函数中变量

垃圾收集：由于v8的优化，闭包访问的变量在调试过程中不可用

### 老旧的var

var没有块级作用域，var允许重新声明，var变量可以在声明前被使用（变量声明提升）（赋值不会），变量也可以不声明直接用

立即调用表达式：写一个函数然后立即执行以此创建块级作用域

### 全局对象

全局对象提供可在任何地方使用的变量和函数。默认情况下，这些全局变量内建于语言或环境中。

在浏览器中，它的名字是 “window”，对 Node.js 而言，它的名字是 “global”，其它环境可能用的是别的名字。

最近，globalThis 被作为全局对象的标准名称加入到了 JavaScript 中，所有环境都应该支持该名称。所有主流浏览器都支持它。

var声明的变量会成为global的属性，函数也有这样的效果

确实需要全局变量时，显式写入window.var属性

### 函数对象，NFE（命名函数表达式）

函数的类型也是对象

函数的名字可以通过属性 “name” 来访问

另一个内建属性 “length”，它返回函数入参的个数（rest 参数不参与计数）

函数也可以添加自定义属性（可以代替闭包，不同之处在于自定义属性可以使用函数对象修改，闭包变量只能用返回的那个函数修改）

命名函数表达式：名字外部不可见，用于函数递归调用

### new Function语法
允许运行时传字符串参数作为函数体动态创建函数

区别：如果我们使用 new Function 创建一个函数，那么该函数的 [[Environment]] 并不指向当前的词法环境，而是指向全局环境。

因此，此类函数无法访问外部（outer）变量，只能访问全局变量。（这是因为代码压缩后变量名可能改变）

### 调度：setTimeout 和 setInterval

这两个函数不在js规范中，但是都支持

调用时延迟后面可以传参（回调函数的参数）

函数返回计时器id，使用clearTimeout解除定时

嵌套的settimeout比serinterval更能保证执行延时的确定（延时不含函数执行时间）

零延时的settimeout会让代码异步执行（脚本代码执行完后开始执行）

零延时实际上不一定为0（在浏览器中）“经过 5 重嵌套定时器之后，时间间隔被强制设定为至少 4 毫秒”。

请注意，所有的调度方法都不能**保证**确切的延时。

### 装饰器模式和转发，call/apply

装饰器（decorator）：一个特殊的函数，它接受另一个函数并改变它的行为。返回新的函数（装饰后的函数）

有一个特殊的内建函数方法 func.call(context, …args)，它允许调用一个显式设置 this 的函数。

我们可以使用 func.apply(this, arguments) 代替 func.call(this, ...arguments)。

call 期望一个参数列表，而 apply 期望一个包含这些参数的类数组对象。

只有一个关于 args 的细微的差别：

Spread 语法 ... 允许将 可迭代对象 args 作为列表传递给 **call**。

apply 只接受 **类数组 args**。

通常，用装饰的函数替换一个函数或一个方法是安全的，除了一件小东西。如果原始函数有属性，例如 func.calledCount 或其他，则装饰后的函数将不再提供这些属性。因为这是装饰器。因此，如果有人使用它们，那么就需要小心。

### 函数绑定

函数提供了一个内建方法 bind，它可以绑定 this。

func.bind(context) 的结果是一个特殊的类似于函数的“外来对象（exotic object）”，它可以像函数一样被调用，并且透明地（transparently）将调用传递给 func 并设定 this=context。

部分应用函数partial：我们不仅可以绑定 this，还可以绑定参数（arguments）

### 深入理解箭头函数 
- 没有 this

- 没有 arguments

- 不能使用 new 进行调用

- 它们也没有 super

## 7. 对象属性配置

### 属性标志和属性描述符

对象属性（properties），除 value 外，还有三个特殊的特性（attributes），也就是所谓的“标志”：

- writable — 如果为 true，则值可以被修改，否则它是只可读的。


- enumerable — 如果为 true，则会被在循环中列出，否则不会被列出。


- configurable — 如果为 true，则此属性可以被删除，这些特性也可以被修改，否则不可以。


当我们用“常用的方式”创建一个属性时，它们都为 true。但我们也可以随时更改它们。

`Object.getOwnPropertyDescriptor` 方法允许查询有关属性的 完整 信息。

语法是：

`let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName)`

- obj:需要从中获取信息的对象。
- propertyName:属性的名称。
- 返回值是一个所谓的“属性描述符”对象：它包含值和所有的标志。

为了修改标志，我们可以使用 `Object.defineProperty`。

语法是：

`Object.defineProperty(obj, propertyName, descriptor)`

- obj，propertyName:要应用描述符的对象及其属性。
- descriptor:要应用的属性描述符对象。

如果该属性存在，defineProperty 会更新其标志。否则，它会使用给定的值和标志创建属性；在这种情况下，如果没有提供标志，则会假定它是 false。

使属性变成不可配置是一条单行道。**我们无法通过 defineProperty 再把它改回来。**

请注意：configurable: false 防止更改和删除属性标志，但是允许更改对象的值。

有一个方法 Object.defineProperties(obj, descriptors)，允许一次定义多个属性。要一次获取所有属性描述符，我们可以使用 Object.getOwnPropertyDescriptors(obj) 方法。


另一个区别是 for..in 会忽略 symbol 类型的和不可枚举的属性，但是 Object.getOwnPropertyDescriptors 返回包含 symbol 类型的和不可枚举的属性在内的 **所有** 属性描述符。

设定一个全局的密封对象
属性描述符在单个属性的级别上工作。

还有一些限制访问整个对象的方法：

`Object.preventExtensions(obj)`
禁止向对象添加新属性。
`Object.seal(obj)`
禁止添加/删除属性。为所有现有的属性设置 configurable: false。
`Object.freeze(obj)`
禁止添加/删除/更改属性。为所有现有的属性设置 configurable: false, writable: false。
还有针对它们的测试：

`Object.isExtensible(obj)`
如果添加属性被禁止，则返回 false，否则返回 true。
`Object.isSealed(obj)`
如果添加/删除属性被禁止，并且所有现有的属性都具有 configurable: false则返回 true。
`Object.isFrozen(obj)`
如果添加/删除/更改属性被禁止，并且所有当前属性都是 configurable: false, writable: false，则返回 true。


### getter and setter
访问器属性由 “getter” 和 “setter” 方法表示。在对象字面量中，它们用 get 和 set 表示：

```js
let obj = {
  get propName() {
    // 当读取 obj.propName 时，getter 起作用
  },
  set propName(value) {
    // 当执行 obj.propName = value 操作时，setter 起作用
  }
}
```

当读取 obj.propName 时，getter 起作用，当 obj.propName 被赋值时，setter 起作用。

### 访问器描述符

访问器属性的描述符与数据属性的不同。

对于访问器属性，没有 value 和 writable，但是有 get 和 set 函数。

所以访问器描述符可能有：

- get —— 一个没有参数的函数，在读取属性时工作，
- set —— 带有一个参数的函数，当属性被设置时调用，
- enumerable —— 与数据属性的相同，
- configurable —— 与数据属性的相同。

## 8. 原型继承

### 原型继承

在 JavaScript 中，对象有一个特殊的隐藏属性 [[Prototype]]（如规范中所命名的），它要么为 null，要么就是对另一个对象的引用。该对象被称为“原型”：

当我们从 object 中读取一个缺失的属性时，JavaScript 会自动从原型中获取该属性。在编程中，这被称为“原型继承”。

属性 [[Prototype]] 是内部的而且是隐藏的，但是这儿有很多设置它的方式。

其中之一就是使用特殊的名字 `__proto__`

引用不能形成闭环。如果我们试图给 `__proto__` 赋值但会导致引用形成闭环时，JavaScript 会抛出错误。
`__proto__` 的值可以是对象，也可以是 null。而其他的类型都会被忽略。

**js不允许多继承**

`__proto__` 是 [[Prototype]] 的 getter/setter

`__proto__` 属性有点过时了。它的存在是出于历史的原因，现代编程语言建议我们应该使用函数 Object.getPrototypeOf/Object.setPrototypeOf 来取代

`__proto__` 去 get/set 原型。稍后我们将介绍这些函数。

根据规范，`__proto__` 必须仅受浏览器环境的支持。但实际上，包括服务端在内的所有环境都支持它，因此我们使用它是非常安全的。

写入不使用原型，原型仅用于读取属性。

对于写入/删除操作可以直接在对象上进行。

无论在哪里找到方法：在一个对象还是在原型中。在一个方法调用中，this 始终是点符号 . 前面的对象。

当继承的对象运行继承的方法时，它们将仅修改自己的状态，而不会修改大对象的状态。

for..in 循环也会迭代继承的属性。我们想排除继承的属性，那么这儿有一个内建方法 obj.hasOwnProperty(key)：如果 obj 具有自己的（非继承的）名为 key 的属性，则返回 true。因此，我们可以过滤掉继承的属性（或对它们进行其他操作）

几乎所有其他键/值获取方法，例如 Object.keys 和 Object.values 等，都会忽略继承的属性。

### F.prototype

如果 F.prototype 是一个对象，那么 new 操作符会使用它为新对象设置 [[Prototype]]

每个函数都有 "prototype" 属性，即使我们没有提供它。

默认的 "prototype" 是一个只有属性 constructor 的对象，属性 constructor 指向函数自身。

### 原生的原型

简短的表达式 obj = {} 和 obj = new Object() 是一个意思，其中 Object 就是一个内建的对象构造函数，其自身的 prototype 指向一个带有 toString 和其他方法的一个巨大的对象。

`obj.__proto__ === Object.prototype`

其他内建对象，像 Array、Date、Function 及其他，都在 prototype 上挂载了方法

**原生的原型是可以被修改的。**例如，我们向 String.prototype 中添加一个方法，这个方法将对所有的字符串都是可用的

### 原型方法，没有 `__proto__` 的对象

在这部分内容的第一章中，我们提到了设置原型的现代方法。

使用 `obj.__proto__` 设置或读取原型被认为已经过时且不推荐使用（deprecated）了（已经被移至 JavaScript 规范的附录 B，意味着仅适用于浏览器）。

现代的获取/设置原型的方法有：

`Object.getPrototypeOf(obj)` —— 返回对象 obj 的 [[Prototype]]。
`Object.setPrototypeOf(obj, proto)` —— 将对象 obj 的 [[Prototype]] 设置为 proto。
`__proto__` 不被反对的唯一的用法是在创建新对象时，将其用作属性：`{ __proto__: ... }`。

虽然，也有一种特殊的方法：

Object.create(proto, [descriptors]) —— 利用给定的 proto 作为 [[Prototype]] 和可选的属性描述来创建一个空对象。

```js
let clone = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
)
```
此调用可以对 obj 进行真正准确地拷贝，包括所有的属性：可枚举和不可枚举的，数据属性和 setters/getters —— 包括所有内容，并带有正确的 [[Prototype]]。


Object.create(null) 创建了一个空对象，这个对象没有原型（[[Prototype]] 是 null）：

它没有继承 __proto__ 的 getter/setter 方法。现在，它被作为正常的数据属性进行处理，因此上面的这个示例能够正常工作。

我们可以把这样的对象称为 “very plain” 或 “pure dictionary” 对象，因为它们甚至比通常的普通对象（plain object）{...} 还要简单。

缺点是这样的对象没有任何内建的对象的方法，例如 toString：

它们通常对关联数组而言还是很友好。

## 9. 类

### Class 基本语法

通过 class 创建的函数具有特殊的内部属性标记 [[IsClassConstructor]]: true。因此，它与手动创建并不完全相同。

编程语言会在许多地方检查该属性。例如，与普通函数不同，必须使用 new 来调用它：

此外，大多数 JavaScript 引擎中的类构造器的字符串表示形式都以 “class…” 开头
```js
class User {
  constructor() {}
}

alert(User); // class User { ... }
```

**类方法不可枚举** 类定义将 "prototype" 中的所有方法的 enumerable 标志设置为 false。

这很好，因为如果我们对一个对象调用 for..in 方法，我们通常不希望 class 方法出现。

类总是使用 use strict。 在类构造中的所有代码都将自动进入严格模式。

类似于命名函数表达式（Named Function Expressions），类表达式可能也应该有一个名字。如果类表达式有名字，那么该名字仅在类内部可见：

**“类字段”**是一种允许添加任何属性的语法。

例如，让我们在 class User 中添加一个 name 属性：我们就只需在表达式中写 “ = ”

类字段的重要区别在于，它们会被挂在实例对象上，而非 User.prototype 上：

类字段提供了另一种非常优雅的语法：
```js
class Button {
  constructor(value) {
    this.value = value;
  }
  click = () => {
    alert(this.value);
  }
}

let button = new Button("hello");

setTimeout(button.click, 1000); // hello
```
类字段 click = () => {...} 是基于每一个对象被创建的，在这里对于每一个 Button 对象都有一个独立的方法，在内部都有一个指向此对象的 this。我们可以把 button.click 传递到任何地方，而且 this 的值总是正确的。在浏览器环境中，它对于进行事件监听尤为有用。

### 类继承
js不允许多继承
类语法不仅允许指定一个类，在 extends 后可以指定任意表达式。例如，一个生成父类的函数调用：

通常，我们不希望完全替换父类的方法，而是希望在父类方法的基础上进行调整或扩展其功能。我们在我们的方法中做一些事儿，但是在它之前或之后或在过程中会调用父类方法。

Class 为此提供了 "super" 关键字。

执行 super.method(...) 来调用一个父类方法。
执行 super(...) 来调用一个父类 constructor（只能在我们的 constructor 中）。
继承类的 constructor 必须调用 super(...)，并且 (!) 一定要在使用 this 之前调用。

**父类构造器总是会使用它自己字段的值，而不是被重写的那一个。**

原因在于字段初始化的顺序。类字段是这样初始化的：

- 对于基类（还未继承任何东西的那种），在构造函数调用前初始化。
- 对于派生类，在 super() 后立刻初始化。

内部探究和HomeObject

### 静态属性和静态方法

我们还可以为整个类分配一个方法。这样的方法被称为 静态的（static）。

在一个类的声明中，它们以 static 关键字开头，如下所示：

在 User.staticMethod() 调用中的 this 的值是类构造器 User 自身（“点符号前面的对象”规则）。

通常，静态方法用于实现属于整个类，但不属于该类任何特定对象的函数。

静态的属性也是可能的，它们看起来就像常规的类属性，但前面加有 static：
```js
class Article {
  static publisher = "Levi Ding";
}

alert( Article.publisher ); // Levi Ding
```
这等同于直接给 Article 赋值

静态属性和方法是可被继承的。

对于 class B extends A，类 B 的 prototype 指向了 A：B.[[Prototype]] = A。因此，如果一个字段在 B 中没有找到，会继续在 A 中查找。

私有的和受保护的属性和方法

在面向对象的编程中，属性和方法分为两组：

内部接口 —— 可以通过该类的其他方法访问，但不能从外部访问的方法和属性。
外部接口 —— 也可以从类的外部访问的方法和属性。
在 JavaScript 中，有两种类型的对象字段（属性和方法）：

公共的：可从任何地方访问。它们构成了外部接口。到目前为止，我们只使用了公共的属性和方法。
私有的：只能从类的内部访问。这些用于内部接口。

**受保护的属性**通常以下划线 _ 作为前缀。

这不是在语言级别强制实施的，但是程序员之间有一个众所周知的约定，即不应该从外部访问此类型的属性和方法。

对于某一属性，让我们将它设为只读。有时候一个属性必须只能被在创建时进行设置，之后不再被修改。要做到这一点，我们只需要设置 getter，而不设置 setter：

**私有属性和方法**应该以 # 开头。它们只在类的内部可被访问。

与受保护的字段不同，私有字段由语言本身强制执行。

### 扩展内建类

内建类却是一个例外。它们相互间不继承静态方法。

### 类检查："instanceof"

obj instanceof Class 算法的执行过程大致如下：

如果这儿有静态方法 Symbol.hasInstance，那就直接调用这个方法：
```js
// 设置 instanceOf 检查
// 并假设具有 canEat 属性的都是 animal
class Animal {
  static [Symbol.hasInstance](obj) {
    if (obj.canEat) return true;
  }
}

let obj = { canEat: true }

alert(obj instanceof Animal) // true：Animal[Symbol.hasInstance](obj) 被调用
```
大多数 class 没有 Symbol.hasInstance。在这种情况下，标准的逻辑是：使用 obj instanceOf Class 检查 Class.prototype 是否等于 obj 的原型链中的原型之一。

这里还要提到一个方法 objA.isPrototypeOf(objB)，如果 objA 处在 objB 的原型链中，则返回 true。所以，可以将 obj instanceof Class 检查改为 Class.prototype.isPrototypeOf(obj)。

Class 的 constructor 自身是不参与检查的！检查过程只和原型链以及 Class.prototype 有关。

创建对象后，如果更改 prototype 属性，可能会导致有趣的结果

可以使用特殊的对象属性 Symbol.toStringTag 自定义对象的 toString 方法的行为。

### Mixin模式

mixin 是一个类，其方法可被其他类使用，而无需继承。

换句话说，mixin 提供了实现特定行为的方法，但是我们不单独使用它，而是使用它来将这些行为添加到其他类中。
在 JavaScript 中构造一个 mixin 最简单的方式就是构造一个拥有实用方法的对象，以便我们可以轻松地将这些实用的方法合并到任何类的原型中。
```js
class User extends Person {
  // ...
}

Object.assign(User.prototype, sayHiMixin)
```
Mixin 可以在自己内部使用继承。

Mixin —— 是一个通用的面向对象编程术语：一个包含其他类的方法的类。

一些其它编程语言允许多重继承。JavaScript 不支持多重继承，但是可以通过将方法拷贝到原型中来实现 mixin。

我们可以使用 mixin 作为一种通过添加多种行为（例如上文中所提到的事件处理）来扩充类的方法。

如果 Mixins 意外覆盖了现有类的方法，那么它们可能会成为一个冲突点。因此，通常应该仔细考虑 mixin 的命名方法，以最大程度地降低发生这种冲突的可能性。

## 10. 错误处理

### 错误处理，"try...catch"

仅对运行时err有效，trycatch同步执行，异步代码不会触发，

对于所有内建的 error，error 对象具有两个主要属性：

- name:Error 名称。例如，对于一个未定义的变量，名称是 "ReferenceError"。
- message:关于 error 的详细文字描述。

还有其他非标准的属性在大多数环境中可用。其中被最广泛使用和支持的是：

- stack:当前的调用栈：用于调试目的的一个字符串，其中包含有关导致 error 的嵌套调用序列的信息。

如果我们不需要 error 的详细信息，catch 也可以忽略它：

throw 操作符会生成一个 error 对象。

语法如下：

`throw <error object>`

技术上讲，我们可以将任何东西用作 error 对象。甚至可以是一个原始类型数据，例如数字或字符串，但最好使用对象，最好使用具有 name 和 message 属性的对象（某种程度上保持与内建 error 的兼容性）。

JavaScript 中有很多内建的标准 error 的构造器：Error，SyntaxError，ReferenceError，TypeError 等。我们也可以使用它们来创建 error 对象。


try...catch 结构可能还有一个代码子句（clause）：finally。

如果它存在，它在所有情况下都会被执行：

try 之后，如果没有 error，
catch 之后，如果有 error。

在下面这个例子中，在 try 中有一个 return。在这种情况下，finally 会在控制转向外部代码前被执行。

没有 catch 子句的 try...finally 结构也很有用。当我们不想在原地处理 error（让它们掉出去吧），但是需要确保我们启动的处理需要被完成时，我们应当使用它。

全局catch

规范中没有相关内容，但是代码的执行环境一般会提供这种机制，因为它确实很有用。例如，Node.JS 有 process.on("uncaughtException")。在浏览器中，我们可以将一个函数赋值给特殊的 window.onerror 属性，该函数将在发生未捕获的 error 时执行。

```js
window.onerror = function(message, url, line, col, error) {
  // ...
}
```

### 自定义Error，扩展error

我们可以正常地从 Error 和其他内建的 error 类中进行继承。我们只需要注意 name 属性以及不要忘了调用 super。

我们可以使用 instanceof 来检查特定的 error。但有时我们有来自第三方库的 error 对象，并且在这儿没有简单的方法来获取它的类。那么可以将 name 属性用于这一类的检查。

包装异常是一项广泛应用的技术：用于处理低级别异常并创建高级别 error 而不是各种低级别 error 的函数。在上面的示例中，低级别异常有时会成为该对象的属性，例如 err.cause，但这不是严格要求的。

## 11. Promise，async/await

### 简介：回调
JavaScript 主机（host）环境提供了许多函数，这些函数允许我们计划 异步 行为（action）—— 也就是在我们执行一段时间后才自行完成的行为。

会导致回调地狱（厄运金字塔）

### Promise
Promise 对象的构造器（constructor）语法如下：
```js
let promise = new Promise(function(resolve, reject) {
  // executor（生产者代码）
})
```

传递给 new Promise 的函数被称为 executor。当 new Promise 被创建，executor 会自动运行。它包含最终应产出结果的生产者代码。

它的参数 resolve 和 reject 是由 JavaScript 自身提供的回调。我们的代码仅在 executor 的内部。

当 executor 获得了结果，无论是早还是晚都没关系，它应该调用以下回调之一：

- `resolve(value)` —— 如果任务成功完成并带有结果 value。
- `reject(error)` —— 如果出现了 error，error 即为 error 对象。

所以总结一下就是：executor 会自动运行并尝试执行一项工作。尝试结束后，如果成功则调用 resolve，如果出现 error 则调用 reject。

由 new Promise 构造器返回的 promise 对象具有以下内部属性：

- state —— 最初是 "pending"，然后在 resolve 被调用时变为 "fulfilled"，或者在 reject 被调用时变为 "rejected"。
- result —— 最初是 undefined，然后在 resolve(value) 被调用时变为 value，或者在 reject(error) 被调用时变为 error。
与最初的 “pending” promise 相反，一个 resolved 或 rejected 的 promise 都会被称为 “settled”。

最重要最基础的一个就是 .then。

语法如下：
```js
promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
)
```
.then 的第一个参数是一个函数，该函数将在 promise resolved 且接收到结果后执行。

.then 的第二个参数也是一个函数，该函数将在 promise rejected 且接收到 error 信息后执行。

如果我们只对 error 感兴趣，那么我们可以使用 null 作为第一个参数：.then(null, errorHandlingFunction)。或者我们也可以使用 .catch(errorHandlingFunction)，其实是一样的：

调用 .finally(f) 类似于 .then(f, f)，因为当 promise settled 时 f 就会执行：无论 promise 被 resolve 还是 reject。

finally 的功能是设置一个处理程序在前面的操作完成后，执行清理/终结。

**finally 处理程序（handler）没有参数**。在 finally 中，我们不知道 promise 是否成功。没关系，因为我们的任务通常是执行“常规”的完成程序（finalizing procedures）。

finally 处理程序将结果或 error “传递”给下一个合适的处理程序。

finally 处理程序也不应该返回任何内容。如果它返回了，返回的值会默认被忽略。

此规则的唯一例外是当 finally 处理程序抛出 error 时。此时这个 error（而不是任何之前的结果）会被转到下一个处理程序。

我们可以对 settled 的 promise 附加处理程序
如果 promise 为 pending 状态，.then/catch/finally 处理程序（handler）将等待它的结果。

有时候，当我们向一个 promise 添加处理程序时，它可能已经 settled 了。在这种情况下，这些处理程序会立即执行

### Promise链

从技术上讲，我们也可以将多个 .then 添加到一个 promise 上。但这并不是 promise 链（chaining）。

确切地说，处理程序返回的不完全是一个 promise，而是返回的被称为 “thenable” 对象 —— 一个具有方法 .then 的任意对象。它会被当做一个 promise 来对待。

使用Promise进行错误处理

如果出现 error，promise 的状态将变为 “rejected”，然后执行应该跳转至最近的 rejection 处理程序。但上面这个例子中并没有这样的处理程序。因此 error 会“卡住”。没有代码来处理它。

在实际开发中，就像代码中常规的未处理的 error 一样，这意味着某些东西出了问题。

在任何情况下我们都应该有 unhandledrejection 事件处理程序（用于浏览器，以及其他环境的模拟），以跟踪未处理的 error 并告知用户（可能还有我们的服务器）有关信息，以使我们的应用程序永远不会“死掉”。

### Promise API

`Promise.resolve(value)` 用结果 value 创建一个 resolved 的 promise。

`Promise.reject(error)` 用 error 创建一个 rejected 的 promise。

`Promise.all(promises)` —— 等待所有 promise 都 resolve 时，返回存放它们结果的数组。如果给定的任意一个 promise 为 reject，那么它就会变成 Promise.all 的 error，所有其他 promise 的结果都会被忽略。

`Promise.allSettled(promises)`（ES2020 新增方法）—— 等待所有 promise 都 settle 时，并以包含以下内容的对象数组的形式返回它们的结果：
status: "fulfilled" 或 "rejected", value（如果 fulfilled）或 reason（如果 rejected）。

`Promise.race(promises)` —— 等待第一个 settle 的 promise，并将其 result/error 作为结果返回。

`Promise.any(promises)`（ES2021 新增方法）—— 等待第一个 fulfilled 的 promise，并将其结果作为结果返回。如果所有 promise 都 rejected，Promise.any 则会抛出 AggregateError 错误类型的 error 实例。

### Promisification(promise化)
```JS
function promisify(f) {
  return function (...args) { // 返回一个包装函数（wrapper-function） (*)
    return new Promise((resolve, reject) => {
      function callback(err, result) { // 我们对 f 的自定义的回调 (**)
        if (err) {
          reject(err);
        } else {
          resolve(result);
        }
      }

      args.push(callback); // 将我们的自定义的回调附加到 f 参数（arguments）的末尾
    
      f.call(this, ...args); // 调用原始的函数
    });
  };
}

// 用法：
let loadScriptPromise = promisify(loadScript);
loadScriptPromise(...).then(...);
```

### 微任务（Microtask）


队列（queue）是先进先出的：首先进入队列的任务会首先运行。
只有在 JavaScript 引擎中没有其它任务在运行时，才开始执行任务队列中的任务。
如果一个 promise 的 error 未被在微任务队列的末尾进行处理，则会出现“未处理的 rejection”。

Promise 处理始终是异步的，因为所有 promise 行为都会通过内部的 “promise jobs” 队列，也被称为“微任务队列”（V8 术语）。

因此，.then/catch/finally 处理程序总是在当前代码完成后才会被调用。

如果我们需要确保一段代码在 .then/catch/finally 之后被执行，我们可以将它添加到链式调用的 .then 中。

### async/await

在函数前面的 “async” 这个单词表达了一个简单的事情：即这个函数总是返回一个 promise。其他值将自动被包装在一个 resolved 的 promise 中。
await只在 async 函数内工作
``let value = await promise```

关键字 await 让 JavaScript 引擎等待直到 promise 完成（settle）并返回结果。

现代浏览器在 modules 里允许顶层的 await

await 接受 “thenables”

要声明一个 class 中的 async 方法，只需在对应方法前面加上 async 即可：

如果一个 promise 正常 resolve，await promise 返回的就是其结果。但是如果 promise 被 reject，它将 throw 这个 error，就像在这一行有一个 throw 语句那样。

## 12. Generator，高级 iteration

### generator

generator 可以按需一个接一个地返回（“yield”）多个值。它们可与 iterable 完美配合使用，从而可以轻松地创建数据流。

generator 函数
要创建一个 generator，我们需要一个特殊的语法结构：`function*`，即所谓的 “generator function”。

它看起来像这样：
```js
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}
```
generator 函数与常规函数的行为不同。在此类函数被调用时，它不会运行其代码。而是返回一个被称为 “generator object” 的特殊对象，来管理执行流程。

一个 generator 的主要方法就是 next()。当被调用时（译注：指 next() 方法），它会恢复上图所示的运行，执行直到最近的 yield <value> 语句（value 可以被省略，默认为 undefined）。然后函数执行暂停，并将产出的（yielded）值返回到外部代码。

next() 的结果始终是一个具有两个属性的对象：

value: 产出的（yielded）的值。
done: 如果 generator 函数已执行完成则为 true，否则为 false。
generator 是可迭代的
当你看到 next() 方法，或许你已经猜到了 generator 是 可迭代（iterable）的。

我们可以使用 for..of 循环遍历它所有的值：

这是因为当 done: true 时，for..of 循环会忽略最后一个 value。因此，如果我们想要通过 for..of 循环显示所有的结果，我们必须使用 yield 返回它们：

对于 generator 而言，我们可以使用 yield* 这个特殊的语法来将一个 generator “嵌入”（组合）到另一个 generator 中：

`yield*` 指令将执行 委托 给另一个 generator。这个术语意味着 yield* gen 在 generator gen 上进行迭代，并将其产出（yield）的值透明地（transparently）转发到外部。就好像这些值就是由外部的 generator yield 的一样。

“yield” 是一条双向路
目前看来，generator 和可迭代对象类似，都具有用来生成值的特殊语法。但实际上，generator 更加强大且灵活。

这是因为 yield 是一条双向路（two-way street）：它不仅可以向外返回结果，而且还可以将外部的值传递到 generator 内。

调用 generator.next(arg)，我们就能将参数 arg 传递到 generator 内部。这个 arg 参数会变成 yield 的结果。

我们可以看到，与常规函数不同，generator 和调用 generator 的代码可以通过在 next/yield 中传递值来交换结果。

要向 yield 传递一个 error，我们应该调用 generator.throw(err)。在这种情况下，err 将被抛到对应的 yield 所在的那一行。

generator.return(value) 完成 generator 的执行并返回给定的 value。


### 异步迭代和 generator

异步迭代允许我们对按需通过异步请求而得到的数据进行迭代。

当值是以异步的形式出现时，例如在 setTimeout 或者另一种延迟之后，就需要异步迭代。

最常见的场景是，对象需要发送一个网络请求以传递下一个值，稍后我们将看到一个它的真实示例。

要使对象异步迭代：

使用 Symbol.asyncIterator 取代 Symbol.iterator。

next() 方法应该返回一个 promise（带有下一个值，并且状态为 fulfilled）。

关键字 async 可以实现这一点，我们可以简单地使用 async next()。

我们应该使用 for await (let item of iterable) 循环来迭代这样的对象。

注意关键字 await。

在 `function*` 前面加上 async。这即可使 generator 变为异步的。

然后使用 for await (...) 来遍历它

## 13. 模块

