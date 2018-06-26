# compose

## 要求

```javascript
const addOne = v => v + 1
const multiplyTen = v => 10 * v
const addTen = v => v + 10
```

要求实现一个函数compose，使得：

```javascript
compose(addOne, multiplyTen, addTen)(1)
```

的结果与

```javascript
addOne(multiplyTen(addTen(1)))
```

的结果一致。


## 分析

这是一道考察函数式编程的题目，redux源码中关于compose函数的实现如下：

```javascript
/**
 * Composes single-argument functions from right to left. The rightmost
 * function can take multiple arguments as it provides the signature for
 * the resulting composite function.
 *
 * @param {...Function} funcs The functions to compose.
 * @returns {Function} A function obtained by composing the argument functions
 * from right to left. For example, compose(f, g, h) is identical to doing
 * (...args) => f(g(h(...args))).
 */

export default function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```
