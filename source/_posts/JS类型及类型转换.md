---
title: JS类型及类型转换
date: 2019-07-29 11:21:07
tags:
---

# JS数据类型
## JS常用的数据类型
* string 字符串
* number 数字
* boolean 布尔
* null 空
* undefined *symbol 独一无二的值
* object 对象（Date, Array, Object, Function.....）
# 类型判断
## typeof获取数据类型
```
typeof "John"           // 返回 string
typeof 3.1              // 返回 number
typeof NaN              // 返回 number
typeof false            // 返回 boolean
typeof symbol()         // 返回symbol
typeof [1,2,3,4]        // 返回 object
typeof {name:'John'}    // 返回 object
typeof new Date()       // 返回 object
typeof function () {}   // 返回 function
typeof myCar            // 返回 undefined 
typeof null             // 返回 object
```
>请注意：
>NaN 的数据类型是 number
>数组(Array)的数据类型是 object
>日期(Date)的数据类型为 object
>null 的数据类型是 object
>未定义变量的数据类型为 undefined
>如果对象是 Array 或 Date，我们就无法通过typeof来判断他们的类型，因为都是返回object。
## 类型判断实例
对数组的判断

```
1、
function isArray(myArray) {
    return myArray.constructor.toString().indexOf("Array") > -1;
}

2、
function isArray(myArray) {
    return Object.prototype.toString.call(myArray) === ‘[object Array]’;
}

3、
myArray instanceof Array

4、
Array.prototype.isPrototypeOf(myArray)

5、
Object.getPrototypeOf(myArray) === Array.prototype

6、
Array.isArray(myArray)
```

## constructor属性
```
"John".constructor              // 返回函数 String() { [native code] }
(3.14).constructor              // 返回函数 Number() { [native code] }
false.constructor               // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor           // 返回函数 Array() { [native code] }
{name:'John'}.constructor       // 返回函数 Object() { [native code] }
new Date().constructor          // 返回函数 Date() { [native code] }
function () {}.constructor      // 返回函数 Function(){ [native code] }
```
>总结:constructor属性返回所有 JavaScript 变量的构造函数。
# JS类型转换
>JavaScript 变量可以转换为新变量或其他数据类型：
>通过使用 JavaScript 函数
>通过 JavaScript 自身自动转换

原始类型转换
* 原始值转对象很简单，原始值通过调用String()，Number()，Boolean()构造函数，转换为他们各自的包装对象。null和undefined属于例外，当将他们用在期望是一个对象的地方都会造成一个类型错误(TypeErroe)异常，而不会执行正常的转换。
* 对象类型转换
* 类型转换表
| 值   | 转字符串   | 转数字   | 转布尔值   | 转对象   | 
|:----|:----|:----|:----|:----|
| undefined   | 'undefined'   | NaN   | FALSE   | throws TypeError   | 
| null   | 'null'   | 0   | FALSE   | throws TypeError   | 
| true   | ‘true'   | 1   | true   | new Boolean(true)   | 
| false   | 'false'   | 0   | false   | new Boolean(false)   | 
| '' (空字符串)   | ''   | 0   | false   | new String('')   | 
| '1.2'(数字字符串)   | '1.2'   | 1.2   | true   | new String('1.2')   | 
| 'shiny'(非数字字符串)   | 'shiny'   | NaN   | true   | new String('shiny')   | 
| 0   | '0'   | 0   | false   | new Number(0)   | 
| Infinity   | 'Infinity'   | Infinity   | true   | new Number(Infinity)   | 
| {} (任意对象)   | '[object Object]'   | NaN   | true   | new Number({})   | 
| []   | ''   | 0   | true   | new Number([])   | 
| [33]   | '33'   | 33   | true   | new Number([33])   | 
| function(){}   | 'function(){}'   | NaN   | true   | new Number(function(){})   | 

## 强制类型转换
```
ToString、String、
ToNumber、Number、ParseInt、ParseFloat
ToBoolean、Boolean
```
## 隐式装箱 toPrimitive
>toPrimitive(input, preferedType?) 指将被调用的指定函数值的属性转换为相对应的原始值。

可选参数PreferredType可以是Number或者String,它只代表了一个转换的偏好,转换结果不一定必须是这个参数所指的类型,但转换结果一定是一个原始值.如果PreferredType被标志为Number,则会进行下面的操作来转换输入的值 (§9.1):
1. 如果输入的值已经是个原始值,则直接返回它.
2. 否则,如果输入的值是一个对象.则调用该对象的valueOf()方法.如果valueOf()方法的返回值是一个原始值,则返回这个原始值.
3. 否则,调用这个对象的toString()方法.如果toString()方法的返回值是一个原始值,则返回这个原始值.
4. 否则,抛出TypeError异常.

如果PreferredType被标志为String,则转换操作的第二步和第三步的顺序会调换.如果没有PreferredType这个参数,则PreferredType的值会按照这样的规则来自动设置:Date类型的对象会被设置为String,其它类型的值会被设置为Number.
实例
```
var obj = {
	toString : function(){
		console.log("toString");
		return {};
	},
	valueOf : function(){
		console.log("valueOf");
		return {};
	}
};
Number(obj);                // 先输出然后valueOf,然后toString,然后报错
String(obj);                // 先输出toString,然后valueOf,然后报错
```
### 加法（+）
有这样的一个加法操作：value1 + value22
在计算这个表达式时,内部的操作步骤是这样的 ：
1. 将两个操作数转换为原始值 (下面是数学表示法,不是JavaScript代码): prim1 := ToPrimitive(value1) prim2 := ToPrimitive(value2) PreferredType被省略,因此Date类型的值采用String,其他类型的值采用Number.
2. 如果prim1或者prim2中的任意一个为字符串,则将另外一个也转换成字符串,然后返回两个字符串连接操作后的结果.
3. 否则,将prim1和prim2都转换为数字类型,返回他们的和.
### 字符串+其他原始类型
字符串在加号运算有最高的优先运算，与字符串相加必定是字符串连接运算。所有的其他原始数据类型转为字符串，可以参考ECMAScript标准中的[ToString](http:////www.ecma-international.org/ecma-262/6.0/#sec-tostring)对照表，以下为一些简单的例子：
```
> '1' + 123
"1123"

> '1' + false
"1false"

> '1' + null
"1null"

> '1' + undefined
"1undefined"
```
### 字符串+其他的非字符串的原始数据类型
* 数字与其他类型作相加时，除了字符串会优先使用字符串连接运算(concatenation)的，其他都要依照数字为优先，所以除了字符串之外的其他原始数据类型，都要转换为数字来进行数学的相加运算。如果明白这项规则，就会很容易的得出加法运算的结果。
* 所有转为数字类型可以参考ECMAScript标准中的[ToNumber](http:////www.ecma-international.org/ecma-262/6.0/#sec-tonumber)对照表，以下为一些简单的例子:
```
> 1 + true //true转为1, false转为0
2

> 1 + null //null转为0
1

> 1 + undefined //undefined转为NaN
NaN
```

### 数字/字符串以外的原始数据类型作加法运算
当数字与字符串以外的，其他原始数据类型直接使用加号运算时，就是转为数字再运算，这与字符串完全无关。
```
> true + true
2

> true + null
1

> undefined + null
NaN
```

### 空数组+空数组
```
[]+[] > ‘’
```

两个数组相加，依然按照valueOf -> toString的顺序，但因为valueOf是数组本身，所以会以toString的返回值才是原始数据类型，也就是空字符串，所以这个运算相当于两个空字符串在相加，依照加法运算规则第2步骤，是字符串连接运算(concatenation)，两个空字符串连接最后得出一个空字符串。
空对象+空对象
* 两个空对象相加，依然按照valueOf -> toString的顺序，但因为valueOf是对象本身，所以会以toString的返回值才是原始数据类型，也就是"[object Object]"字符串，所以这个运算相当于两个"[object Object]"字符串在相加，依照加法运算规则第2步骤，是字符串连接运算(concatenation)，最后得出一个"object Object”字符串。
* 但是这个结果有异常，上面的结果只是在Chrome浏览器上的结果(v55)，怎么说呢？
* 有些览器例如Firefox、Edge浏览器会把{} + {}直译为相当于+{}语句，因为它们会认为以花括号开头({)的，是一个区块语句的开头，而不是一个对象字面量，所以会认为略过第一个{}，把整个语句认为是个+{}的语句，也就是相当于强制求出数字值的Number({})函数调用运算，相当于Number("[object Object]”)运算，最后得出的是NaN。
```
{}+{}    >   ‘[object Objecet][object Object]’
```

### 空对象+空数组
```
{}+[]    >   0
[]+{}   > “[object Object]”
```

* 上面同样的把{}当作区块语句的情况又会发生，不过这次所有的浏览器都会有一致结果，如果{}(空对象)在前面，而{}(空数组)在后面时，前面(左边)那个运算元会被认为是区块语句而不是对象字面量。
* 所以{}+[]相当于+[]语句，也就是相当于强制求出数字值的Number([])运算，相当于Number(‘’)运算，最后得出的是0数字。
* 特别注意: 所以如果第一个(前面)是{}时，后面加上其他的像数组、数字或字符串，这时候加号运算会直接变为一元正号运算，也就是强制转为数字的运算。这是个陷阱要小心。
### Date对象
Date对象的valueOf与toString的两个方法的返回值:
>1、valueOf方法返回值: 给定的时间转为UNIX时间(自1 January 1970 00:00:00 UTC起算)，但是以微秒计算的数字值
>2、toString方法返回值: 本地化的时间字符串
* DateDate对象上面有提及是首选类型为”字符串"的一种异常的对象，这与其他的对象的行为不同(一般对象会先调用valueOf再调用toString)，在进行加号运算时时，它会优先使用toString来进行转换，最后必定是字符串连接运算(concatenation)，例如以下的结果:
```
1+(new Date()) > "1Sun Nov 27 2016 01:09:03 GMT+0800 (CST)"
+ new Date() > 1551678691984
```
实例2
```
Array.prototype[Symbol.toPrimitive] = function(hint){
	switch(hint){
		case 'number' :
			return 123;
		case 'string' :
			return 'hello world!';
		case 'default' : 
			return 'default';
		default :
			throw new Error();
	} 
}

String([])
> "hello world!"

Number([])
> 123

[]+[]
> "defaultdefault"

[]+{}
> "default[object Object]"

{}+[]
> 123

[] + Number('123')
> "default123"

[] - Number(2)
> 121

[] + Object({a: 1})
> "default[object Object]"

[] + Object(1)
> "default1"

[] + Date(2)
> "defaultFri Mar 08 2019 10:07:33 GMT+0800 (中国标准时间)"

[] + Date(new Date())
> "defaultFri Mar 08 2019 10:07:40 GMT+0800 (中国标准时间)"

```

