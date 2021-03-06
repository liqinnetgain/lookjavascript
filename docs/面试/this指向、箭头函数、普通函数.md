# this指向、箭头函数、普通函数

箭头函数看上去是匿名函数的一种简写，但实际上，箭头函数和匿名函数有个明显的区别：箭头函数内部的this是词法作用域，由上下文确定。

由于JavaScript函数对`this`绑定的错误处理，下面的例子无法得到预期结果：

```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = function () {
            return new Date().getFullYear() - this.birth; // this指向window或undefined
        };
        return fn();
    }
};
```

现在，箭头函数完全修复了`this`的指向，`this`总是指向词法作用域，也就是外层调用者`obj`：

```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); // 25
```

如果使用箭头函数，以前的那种hack写法：`var that = this;`——就不再需要了。

由于`this`在箭头函数中已经按照词法作用域绑定了，所以，用`call()`或者`apply()`调用箭头函数时，无法对`this`进行绑定，即传入的第一个参数被忽略：

```javascript
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是1990
        return fn.call({birth:2000}, year);
    }
};
obj.getAge(2015); // 25
```

再举一个例子：

```javascript
function make () {  
  return ()=>{  
    console.log(this);  
  }  
}  
var testFunc = make.call({ name:'foo' });  
testFunc(); // {name: "foo"}
testFunc.call({ name:'bar' }); // {name: "foo"}
```

可以看到箭头函数在定义之后，`this`就不会发生改变了，无论用什么样的方式调用它，`this`都不会改变；

所以箭头函数没有它自己的`this`值，箭头函数内的`this`值继承自外围作用域。在箭头函数中调用`this`时，仅仅是简单地沿着作用域链向上寻找，找到最近的一个`this`拿来使用：

```javascript
{
  ...  
  addAll: function addAll(pieces) {  
    var self = this;  
    _.each(pieces, function (piece) {  
      self.add(piece);  
    });  
  },
  ...  
}  
```

在这里，你希望在内层函数里写的是`this.add(piece)`，不幸的是，内层函数并未从外层函数继承`this`的值。在内层函数里，`this`会是`window`或`undefined`，临时变量`self`用来将外部的`this`值导入内部函数。

另一种方式是在内部函数上执行`.bind(this)`。

两种方法都不甚美观。这时候就可以使用箭头函数来达到要求。


## Reference

[箭头函数和普通函数的区别](https://blog.csdn.net/yintianqin/article/details/76091189)
[箭头函数 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001438565969057627e5435793645b7acaee3b6869d1374000)
