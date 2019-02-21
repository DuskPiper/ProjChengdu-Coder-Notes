# JavaScript Basics 基础

### number

- JS的数字是双精度64位值

- 使用`parseInt()`或 `+` 来转换string为number

  ```javascript
  parseInt('123', 10); // 第二个参数是进制
  parseInt('0x0023');
  + '0x0023';
  ```

- `NaN`表示非数字，`Infinity`和`-Infinity`表示表示无限大和无限小

  ```javascript
  isFinite(1 / 0); // false
  isNaN(NaN); // true
  ```

### string

- `string.length`
- `string.charAt(index)`
- `string.replace('original', 'target')`
- `string.toUpperCase()`

### boolean

```javascript
Boolean(''); // false
Blooean(123); // true
```

### Variables 变量

- `let`: block-wise, 是推荐的定义变量的方法。

  ```javascript
  let a;
  let name = 'DuskPiper';
  ```

- `const`: block-wise, 定义不可变值的变量。
- `var`: 没有域限制，默认是 `undefined`。是极为常用的、也是传统的定义手法，但能用`let`的话还是避免用`var`。

### Operators 操作符

- `+` &`+=`

- ```javascript
  x += 5 // x = x + 5
  'Dusk' + 'Piper' // 'DuskPiper
  '3' + 4 + 5 // "345"
  3 + 4 + '5' // "75"
  ```

- `==`&`===`

  ```javascript
  123 == '123' // true, auto type coercion
  123 === '123' // false
  1 == true // true
  1 === true // false
  ```

- `&&`&`||`

### Control Structures

- if - else

  ```javascript
  if (...) {
    ...
  } else if () {
    ...   
  } else {
    ...
  }
  ```

- while

  ```javascript
  while () {
   ...
  }
  
  do {
   ...
  } while ()
  ```

- for

  ```javascript
  for (var i = 0; i < 10; i ++) {
    ...
  }
      
  for (let value of array) {
    ...
  }
  
  for (let property in object) {
    // do something with object property
  }
  ```

- conditions

  ```javascript
  var allowed = (age >= 18) ? true : false;
  ```

- switch

  ```javascript
  switch (action) {
    case 'draw':
      ...
      break;
    case 'eat':
      ...
      break;
    default:
      ...
  }
  ```

### Objects 对象

- 创建对象

  ```javascript
  var apple = new Fruit();
  var apple = {
    name: 'Apple',
    details: {
      color: 'red',
      size: 'small'
    }
  };
  ```

- 访问对象性质

  ```javascript
  apple.details.size; // 'small'
  apple['details']['size']; // 'small'
  ```

- 对象的函数式定义

  ```javascript
  function Agent(name, level) {
    this.name = name;
    this.level = level;
  }
  
  var piper = new Agent('DuskPiper', '16');
  ```

### Array

| Method name                                          | Description                                                  |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| `a.toString()`                                       | Returns a string with the `toString()` of each element separated by commas. |
| `a.toLocaleString()`                                 | Returns a string with the `toLocaleString()` of each element separated by commas. |
| `a.concat(item1[, item2[, ...[, itemN]]])`           | Returns a new array with the items added on to it.           |
| `a.join(sep)`                                        | Converts the array to a string — with values delimited by the `sep` param |
| `a.pop()`                                            | Removes and returns the last item.                           |
| `a.push(item1, ..., itemN)`                          | Appends items to the end of the array.                       |
| `a.reverse()`                                        | Reverses the array.                                          |
| `a.shift()`                                          | Removes and returns the first item.                          |
| `a.slice(start[, end])`                              | Returns a sub-array.                                         |
| `a.sort([cmpfn])`                                    | Takes an optional comparison function.                       |
| `a.splice(start, delcount[, item1[, ...[, itemN]]])` | Lets you modify an array by deleting a section and replacing it with more items. |
| `a.unshift(item1[, item2[, ...[, itemN]]])`          | Prepends items to the start of the array.                    |

### functions 函数

- 普通定义

  ```javascript
  function add(x, y) {
    return x + y;
  }
  ```

- 定义带`…args`参数的函数

  ```javascript
  function sum(...args) {
    var sum = 0;
    for (let val of args) {
      sum += val;
    }
    return sum;
  }
  ```

- Anonymous Functions 匿名函数

  ```javascript
  var ans = function(x) {return x + 1};
  ```

  ```javascript
  (function(x, y) { // 第一个括号定义了函数
    alert(x + y);
  })(2, 3); // 第二个括号传参
  ```

## 参考资料

- [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

