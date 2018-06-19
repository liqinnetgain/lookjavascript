# var、let、const和块级作用域

## 重点

- ES6之前，JS中有全局作用域和函数作用域，从ES6开始，新增了块级作用域，块级作用域由`{}`包围，if/for语句里的`{}`也是块级作用域；
- var定义的变量，没有块的概念，可以跨块访问，不能跨函数访问；
- let定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问；
- const定义常量，使用时必须初始化（即必须赋值），只能在块作用域里访问，而且不能修改。


## 比较老的面试题是这样的：

请问下面会输入什么内容：

case 1:

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i)
  }, 0)
}
console.log(i)
```

答案是会依次输出4次`3`，因为`ES5`中花括号本身是不会产生块级作用域的。如果改成下面这样：

case 2:

```javascript
for (let i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i)
  }, 0)
}
console.log(i)
```

因为用了let，所以这是`ES6`环境了，花括号会产生块级作用域，也就是说上面的代码跟下面的代码执行结果是一样的：

case 3:

```javascript
for (let i = 0; i < 3; i++) {
  (function (d) {
    setTimeout(function () {
      console.log(d)
    }, 0)
  })(i)
}
console.log(i)
```

所以case2和case3一样，会先执行最下面的`console.log(i)`，这个时候for循环已经执行完毕，i的值是3，所以会先输出3，然后会执行那些`setTimeout`里的语句，依次输出0、1、2。


## 改版后的题

请问下面的情况会输出什么？

case 4:

```javascript
function test (arr) {
  var len = arr.length;
  var i = 0;
  var results = [];
  for (; i < len; i++) {
    results[i] = function () {
      console.log(i)
      return arr[i]
    }
  }
  return results
}
var hello = test([1, 2, 3, 4, 5])
console.log(hello[0]())
```

答案：会输出`5`和`undefined`。
