# JS类型转换

使用`==`而非`===`时，会有隐性转换的过程，由于该过程繁杂易错，应尽可能使用全等符号`===`进行判断。


## 显示转换


### 一、Number(mix)：转换为数值类型

1、如果是布尔值，true和false分别被转换为1和0。

2、如果是数字值，返回本身。

3、如果是null，返回0。

4、如果是undefined，返回NaN。

5、如果是字符串，遵循以下规则：
  - 如果字符串中只包含数字，则将其转换为十进制（忽略前导0）
  - 如果字符串中包含有效的浮点格式，将其转换为浮点数值（忽略前导0）
  - 如果是空字符串，将其转换为0
  - 如果字符串中包含非以上格式，则将其转换为NaN
  - 如果是对象，则调用对象的valueOf()方法，然后依据前面的规则转换返回的值。<del>如果转换的结果是NaN，则调用对象的toString()方法，再次依照前面的规则转换返回的字符串值。</del>

```javascript
Number('test1') // NaN
Number('1test') // NaN
Number('1e0') // 1
Number('1e0') // 1
Number({ valueOf: function () { return '8' } }) // 8
Number({ valueOf: function () { return null } }) // 0
var obj1 = {
  valueOf: function () { return void 0 },
  toString: function () { return '3' }
}
Number(obj1) // NaN
var obj2 = {
  valueOf: function () { return 'test2' },
  toString: function () { return '3' }
}
Number(obj2) // NaN
```


### 二、parseInt(string, radix)：转换为数值类型（整数）

1、忽略字符串前面的空格，直至找到第一个非空字符

2、如果第一个字符不是数字符号或者<u>正、</u>负号，返回NaN

3、如果第一个字符是数字，则继续解析直至字符串解析完毕或者遇到一个非数字符号为止

4、如果上步解析的结果<del>以0开头，则将其当作八进制来解析；</del>如果以<u>0</u>x开头，则将其当作十六进制来解析

5、如果指定radix参数，则以radix为基数进行解析

```javascript
parseInt('.') // NaN
parseInt('.1') // NaN
parseInt('2.1') // 2
parseInt('test') // NaN
parseInt(4) // 4
parseInt('04') // 4
parseInt('') // NaN
parseInt('2d') // 2
parseInt('020dd') // 20
parseInt('09d') // 9
parseInt('x6') // NaN
parseInt('0xd', 8) // 0
parseInt('0xd') // 13
parseInt('0xd', 16) // 13
```


### 三、parseFloat(string)：转换为数值类型（浮点数）

1、它的规则与parseInt基本相同，但也有点区别：字符串中第一个小数点符号是有效的，另外parseFloat会忽略所有前导0，如果字符串包含一个可解析为整数的数，则返回整数值而不是浮点数值。

```javascript
parseFloat('.') // NaN
parseFloat('.1') // 0.1
parseFloat('2.1') // 2.1
parseFloat(2.1) // 2.1
```


### 四、toString(radix)：转换为字符串类型

除undefined和null之外的所有类型的值都具有toString()方法，其作用是返回对象的字符串表示。

1、Array：将Array的元素转换为字符串。结果字符串由逗号分隔，且连接起来。

2、Boolean：如果Boolean值是true，则返回`"true"`。否则，返回`"false"`。

3、Date：返回日期的文字表示法。

4、Error	：返回一个包含相关错误信息的字符串。

5、Function：返回如下格式的字符串，其中`functionname`是被调用`toString`方法函数的名称：`"function functionname( ) { [native code] }"`。

6、Number：返回数字的文字表示。

7、String：返回 String 对象的值。

8、默认：返回`"[object objectname]"`，其中`objectname`是对象类型的名称。

```javascript
[1, 2, 3].toString() // "1,2,3"
true.toString() // "true"
new Date().toString() // e.g. "Tue Jun 19 2018 14:13:01 GMT+0800 (中国标准时间)"
new Error('message').toString() // "Error: message"
parseInt.toString() // "parseInt.toString()"
(1).toString() // "1"
new Object().toString() // "[object Object]"
```

### 五、String(mix)：转换为字符串类型

1、如果有toString()方法，则调用该方法（不传递radix参数）并返回结果

2、如果是null，返回`"null"`

3、如果是undefined，返回`"undefined"`


### 六、Boolean(mix)：转换为布尔类型

以下值会被转换为`false`：`false`、`""`、`0`、`NaN`、`null`、`undefined`，其余任何值都会被转换为`true`。

```javascript
Boolean(0) // false
Boolean('0') // true
```


## 隐式转换

在某些情况下，即使我们不提供显示转换，Javascript也会进行自动类型转换，主要情况有：


### 一、用于检测是否为非数值的函数：isNaN(mix)

isNaN()函数，经测试发现，该函数会尝试将参数值用Number()进行转换，如果结果为“非数值”则返回true，否则返回false。


### 二、递增递减操作符（包括前置和后置）、一元正负符号操作符

这些操作符适用于任何数据类型的值，针对不同类型的值，该操作符遵循以下规则（经过对比发现，其规则与Number()规则基本相同）：

1、如果是包含有效数字字符的字符串，先将其转换为数字值（转换规则同Number()），在执行加减1的操作，字符串变量变为数值变量。

2、如果是不包含有效数字字符的字符串，将变量的值设置为NaN，字符串变量变成数值变量。

3、如果是布尔值false，先将其转换为0再执行加减1的操作，布尔值变量编程数值变量。

4、如果是布尔值true，先将其转换为1再执行加减1的操作，布尔值变量变成数值变量。

5、如果是浮点数值，执行加减1的操作。

6、如果是对象，先调用对象的valueOf()方法，然后对该返回值应用前面的规则。如果结果是NaN，则调用toString()方法后再应用前面的规则。对象变量变成数值变量。

```javascript
var a = '1'
var b = '02dd'
var c = ''
var d = false
var e = 22.5
var f = true
++a // 2
++b // NaN
++c // 1
++d // 1
++e // 23.5
++f // 2
-false // -0
+false // 0
-true // -1
+true // 1
+new Date() // 1529390879570
```


### 三、加法运算操作符

加号运算操作符在Javascript也用于字符串连接符，所以加号操作符的规则分两种情况：

1、如果两个操作值都是数值，其规则为：

  - 如果一个操作数为`NaN`，则结果为`NaN`
  
  - 如果是`Infinity+Infinity`，结果是`Infinity`
  
  - 如果是`-Infinity+(-Infinity)`，结果是`-Infinity`
  
  - 如果是`Infinity+(-Infinity)`，结果是`NaN`
  
  - 如果是`+0+(+0)`，结果为`+0`
  
  - 如果是`(-0)+(-0)`，结果为`-0`
  
  - 如果是`(+0)+(-0)`，结果为`+0`

2、如果有一个操作值为字符串，则：

  - 如果两个操作值都是字符串，则将它们拼接起来
  
  - 如果只有一个操作值为字符串，则将另外操作值转换为字符串，然后拼接起来
  
  - 如果一个操作数是对象、数值或者布尔值，则调用`toString()`方法取得字符串值，然后再应用前面的字符串规则。对于`undefined`和`null`，分别调用`String()`显式转换为字符串。
  
  - 可以看出，加法运算中，如果有一个操作值为字符串类型，则将另一个操作值转换为字符串，最后连接起来。


### 四、乘除、减号运算符、取模运算符

这些操作符针对的是运算，所以他们具有共同性：如果操作值之一不是数值，则被隐式调用`Number()`函数进行转换。具体每一种运算的详细规则请参考ECMAScript中的定义。


### 五、逻辑操作符（!、&&、||）

1、逻辑非（`!`）操作符首先通过`Boolean()`函数将它的操作值转换为布尔值，然后求反。

2、逻辑与（`&&`）操作符，如果一个操作值不是布尔值时，遵循以下规则进行转换：
  - 如果第一个操作数经`Boolean()`转换后为`true`，则返回第二个操作值，否则返回第一个值（不是`Boolean()`转换后的值）
  - 如果有一个操作值为`null`，返回`null`
  - 如果有一个操作值为`NaN`，返回`NaN`
  - 如果有一个操作值为`undefined`，返回`undefined`
  
3、逻辑或（`||`）操作符，如果一个操作值不是布尔值，遵循以下规则：
  - 如果第一个操作值经`Boolean()`转换后为`false`，则返回第二个操作值，否则返回第一个操作值（不是`Boolean()`转换后的值）
  - 对于`undefined`、`null`和`NaN`的处理规则与逻辑与（`&&`）相同


### 六、关系操作符（<, >, <=, >=）

与上述操作符一样，关系操作符的操作值也可以是任意类型的，所以使用非数值类型参与比较时也需要系统进行隐式类型转换：

1、如果两个操作值都是数值，则进行数值比较

2、如果两个操作值都是字符串，则比较字符串对应的字符编码值

3、如果只有一个操作值是数值，则将另一个操作值转换为数值，进行数值比较

4、如果一个操作数是对象，则调用valueOf()方法（如果对象没有valueOf()方法则调用toString()方法），得到的结果按照前面的规则执行比较

5、如果一个操作值是布尔值，则将其转换为数值，再进行比较

注：NaN是非常特殊的值，它不和任何类型的值相等，包括它自己，***同时它与任何类型的值比较大小时都返回false***。


### 七、相等操作符（==）

相等操作符会对操作值进行隐式转换后进行比较：

1、如果一个操作值为布尔值，则在比较之前先将其转换为数值

2、如果一个操作值为字符串，另一个操作值为数值，则通过Number()函数将字符串转换为数值

3、如果一个操作值是对象，另一个不是，则调用对象的valueOf()方法，得到的结果按照前面的规则进行比较

4、null与undefined是相等的

5、如果一个操作值为NaN，则相等比较返回false

6、如果两个操作值都是对象，则比较它们是不是指向同一个对象

```javascript
NaN == NaN // false
{} == {} // false
```

## Reference

- [JS类型转换（强制和自动的规则）](https://www.cnblogs.com/Juphy/p/7085197.html)