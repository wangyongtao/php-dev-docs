JavaScript基础知识.md


JavaScript® （JS) 是一门轻量的、解释型的、将函数视为一级公民的程序设计语言。
它是最为出名的网页脚本语言，但也在很多非网页环境中运用，例如 node.js 和 Apache CouchDB。
它是一种基于原型的、多范式的动态脚本语言，并且支持面向对象、命令式编程风格和声明式（如：函数式编程）编程风格。

JavaScript 的标准就是 ECMAScript。
截至 2012 年为止，所有的主流浏览器都完整的支持  ECMAScript 5.1，旧式的浏览器至少支持 ECMAScript 3 标准。
在2015年6月17日，ECMA国际组织发布了ECMAScript的第六个版本，该版本正式名称为ECMAScript 2015，但通常被称为ECMAScript 6或者ES6。

JavaScript 与 Java 编程语言是两个不同的概念。虽然“Java”和“JavaScript”都是 Oracle 公司在美国和其他国家注册（或未注册）的商标，但是这两门语言在语法、语义与用途方面有很大不同。


HTML是用来存储网页内容的，CSS是用来定义这些内容的显示样式的，而JavaScript是用来创造丰富的页面效果或者网页应用的。
但是，如果从浏览器的范畴去理解“JavaScript”这个术语，它包含了截然不同的两个方面。
一个是JavaScript的核心语言（ECMAScript），另一个是DOM（文档对象模型）。




typeof, null, 和 undefined


## typeof

typeof 操作符返回一个字符串,表示未经求值的操作数(unevaluated operand)的类型。

语法：typeof operand
参数：operand 是一个表达式，表示对象或原始值，其类型被返回。
描述E：此表总结了 typeof 可能的返回值。

| 类型                 | 结构                         |
| -------------------- | -------------------------- |
| Undefined                                | `"undefined"` |
| Null                                     | `"object" `(见下方) |
| 布尔值                                      | `"boolean"` |
| 数值                                       | `"number"` |
| 字符串                                      | `"string"` |
| Symbol (ECMAScript 6 新增)                 | `"symbol"`      |
| 宿主对象(JS环境提供的，比如浏览器) | *Implementation-dependent* |
| 函数对象 (implements [[Call]] in ECMA-262 terms) | `"function"`|
| 任何其他对象                                   | `"object"`|

实例：

```
typeof "John"                // 返回 string
typeof ""                    // 返回 string
typeof 3.14                  // 返回 number
typeof true                  // 返回 boolean
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
```
> Notes:   
> 在JavaScript中，数组是一种特殊的对象类型。 因此 typeof [1,2,3,4] 返回 object。 


## Null

null 是一个 JavaScript 字面量，表示空值（null or an "empty" value），表示 "什么都没有"。即没有对象被呈现（no object value is present）。
null 是一个只有一个值的特殊类型。表示一个空对象引用。它是 JavaScript 的原始值之一。
null 是一个字面量（而不是全局对象的一个属性，undefined 是）

> Note: 用 typeof 检测 null 返回是object。  
> Note: ECMAScript 有 5 种原始类型（primitive type），即 Undefined、Null、Boolean、Number 和 String。  

实例

```
可以设置为 null 来清空对象:
var person = null;     // Value is null, but type is still an object

也可以设置为 undefined 来清空对象:
var person = undefined; // 值为 undefined, type is undefined
```

## Undefined

undefined有多重角色,通常情况下,我们所说的undefined都指的是全局对象的一个属性"undefined".

在 JavaScript 中, undefined 是一个没有设置值的变量。typeof 一个没有值的变量会返回 undefined。

一个未初始化的变量的值为undefined.
一个没有传入实参的形参变量的值为undefined.
如果一个函数什么都不返回,则该函数默认返回undefined.


> 在JavaScript中,undefined这个词有多重含义:    
> (1) 首字母大写的Undefined表示的是一种数据类型;
> (2) 小写的undefined表示的是属于这种数据类型的唯一的一个值; 
> 但这两种undefined都只能存在于文档或规范中,不能存在于JavaScript代码中.  
> 
> 在JavaScript代码中,我们看到的undefined最有可能是全局对象的一个属性,该属性的初始值是就是前面所说的原始值undefined.    
> 还有种情况就是,这个undefined是个局部变量,就像其他普通变量一样,没有任何特殊性,它的值不一定是undefined,但通常情况下都是的.  
> 
> 下面我们所说的undefined,都指的是window.undefined这个属性.  



实例:

```
var person; // Value is undefined, type is undefined

可以使用严格相等运算符来判断一个值是否是undefined:
var x;
if ( x === undefined ) {
   // 执行到这里
} else {
   // 不会执行到这里
}
```

这里必须使用严格相等运算符===,而不能使用普通的相等运算符==.  
因为x == undefined成立还可能是因为x为null,在JavaScript中null== undefined是返回true的.

另外,还可以使用typeof来判断:

```
var x;
if (typeof x === 'undefined') {
   // 执行到这里
}
```

任何变量都可以通过设置值为 undefined 来清空。 类型为 undefined.
实例
person = undefined;          // 值为 undefined, type is undefined


## NaN 值 

NaN 是一个全局对象的属性，表示 Not-A-Number 的值，是一个特殊值。

在编码很少直接使用到 NaN。通常都是在计算失败时，作为 Math 的某个方法的返回值出现的（例如：Math.sqrt(-1)）或者尝试将一个字符串解析成数字但失败了的时候（例如：parseInt("blabla")）。


NaN 不等于自己, 即 NaN == NaN 返回的是 false 
判断一个值是否是 NaN， 必须使用 Number.isNaN() 或 isNaN() 函数

isNaN() 函数用于检查其参数是否是非数字值。
isNaN() 函数通常用于检测 parseFloat() 和 parseInt() 的结果，以判断它们表示的是否是合法的数字。当然也可以用 isNaN() 函数来检测算数错误，比如用 0 作除数的情况。
如果isNaN函数的参数不是Number类型, isNaN()会首先尝试将这个参数转换为数值，然后才会对转换后的结果是否是NaN进行判断。

```
isNaN(123);   // 返回 false
isNaN(-1.23); // 返回 false
isNaN(5-2);   // 返回 false
isNaN(0);     // 返回 false
isNaN("0");   // 返回 false
isNaN(true);  // 返回 false: 因为Number(true)是1 
isNaN(false); // 返回 false: 因为Number(false)是0 
isNaN(null);  // 返回 false: 因为Number(null)是0 

isNaN("Hello");      // 返回 true
isNaN("2016/07/11"); // 返回 true
isNaN(NaN);       // 返回 true
isNaN(undefined); // 返回 true
isNaN({});        // 返回 true

// dates
isNaN(new Date());                // 返回 false
isNaN(new Date().toString());     // 返回 true
```


undefined 和 Null 的区别

```
typeof undefined    // 返回undefined
typeof null         // 返回object (bug in ECMAScript, should be null)
null === undefined  // 返回false
null == undefined   // 返回true
```


underfined和"undefined"的区别:

undefined是JavaScript提供的一个"关键字"，而"undefined"就是一个字符串，只是字符串的内容长得和undefined一样而已。

```
typeof undefined    // 返回undefined
typeof "undefined"  // 返回string
```



参考链接：

http://www.w3school.com.cn/jsref/jsref_isNaN.asp  
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN  
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined  

