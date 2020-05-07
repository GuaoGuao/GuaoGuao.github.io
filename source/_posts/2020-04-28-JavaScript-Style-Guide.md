---
title: Airbnb js编码规范
date: 2020-04-28 18:44:45
tags:
---

#  Airbnb js编码规范

最近感觉写代码越写越乱，碰巧搜到了一个Google的代码规范，感觉也应该找一个差不多的看看了，在github上搜到了[这个](https://github.com/airbnb/javascript#classes--constructors)，还有[中文版](https://github.com/BingKui/javascript-zh)，有快10w的star了，里面包含了对es_lint的解读，找找自己不知道的地方记下来

## 引用
### 避免使用var

尽量用const和let代替var，因为const避免你的值被修改，let是块级作用域，优于var的函数级作用域

## 对象

* 有动态属性名就用动态属性名的方法定义对象
    
  这样保证你的对象属性都定义在一个地方，像是这样↓

```JavaScript
      function getKey(k) {
        return `a key named ${k}`;
      }

      // bad
      const obj = {
        id: 5,
        name: 'San Francisco',
      };
      obj[getKey('enabled')] = true;

      // good
      const obj = {
        id: 5,
        name: 'San Francisco',
        [getKey('enabled')]: true,
      };
```

* 使用对象方法的缩写

```JavaScript
      // bad
      const atom = {
        value: 1,

        addValue: function (value) {
          return atom.value + value;
        },
      };

      // good
      const atom = {
        value: 1,

        addValue(value) {
          return atom.value + value;
        },
      };
```
* 使用属性名缩写

  因为它短
```JavaScript
      const lukeSkywalker = 'Luke Skywalker';

      // bad
      const obj = {
        lukeSkywalker: lukeSkywalker,
      };

      // good
      const obj = {
        lukeSkywalker,
      };
```

* 只用引号标注无效标识符的属性

  考虑到编辑器能把属性名高亮，加引号就没了

```JavaScript
      // bad
      const bad = {
        'foo': 3,
        'bar': 4,
        'data-blah': 5,
      };

      // good
      const good = {
        foo: 3,
        bar: 4,
        'data-blah': 5,
      };
```

* 不要直接调用Object.prototype的方法，如 hasOwnProperty 、 propertyIsEnumerable 和 isPrototypeOf

  因为有的对象可能调用时出现问题，如`{ hasOwnProperty: false }` 或者这样创建的空对象`Object.create(null)`

```JavaScript
      // bad
      console.log(object.hasOwnProperty(key));

      // good
      console.log(Object.prototype.hasOwnProperty.call(object, key));

      // best
      const has = Object.prototype.hasOwnProperty; // 在模块范围内的缓存中查找一次
      /* or */
      import has from 'has'; // https://www.npmjs.com/package/has
      // ...
      console.log(has.call(object, key));
```

* 尽量使用对象扩展运算符`...`而不是`Object.assign`

  rest操作符和spread操作符都是三个点，rest是把逗号隔开的值序列`组合`成一个数组，spread是`展开`成几个属性。需要根据上下文环境判断

```JavaScript
      // very bad
      const original = { a: 1, b: 2 };
      const copy = Object.assign(original, { c: 3 }); // 变异的 `original` ಠ_ಠ
      delete copy.a; // 这....

      // bad
      const original = { a: 1, b: 2 };
      const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

      // good
      const original = { a: 1, b: 2 };
      const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

      const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```

## 数组

* 使用数组展开方法 `...` 来拷贝数组。

* 如果数组有多行，则在开始的时候换行，然后在结束的时候换行。
```JavaScript
      // bad
      const arr = [
        [0, 1], [2, 3], [4, 5],
      ];

      const objectInArray = [{
        id: 1,
      }, {
        id: 2,
      }];

      const numberInArray = [
        1, 2,
      ];

      // good
      const arr = [[0, 1], [2, 3], [4, 5]];

      const objectInArray = [
        {
          id: 1,
        },
        {
          id: 2,
        },
      ];

      const numberInArray = [
        1,
        2,
      ];
```

* 访问对象的多个属性 或者 数组多项 的时候 使用对象的解构

  解构可以避免为这些属性创建临时引用

```JavaScript
      // 对象解构
      // bad
      function getFullName(user) {
        const firstName = user.firstName;
        const lastName = user.lastName;

        return `${firstName} ${lastName}`;
      }
      // good
      function getFullName({ firstName, lastName }) {
        return `${firstName} ${lastName}`;
      }
      // 属性解构
      const arr = [1, 2, 3, 4];
      // bad
      const first = arr[0];
      const second = arr[1];
      // good 
      const [first, second] = arr;
```
## 字符串

* 使用单引号定义

* 超过100个字符的就不要换行了，不管是用`+`还是用`\`，都不要

* 使用字符串模板，即``

## 方法

* 使用命名的函数表达式代替函数声明

    这里翻译里用了一个奇怪的说法，说函数声明是挂起的，其实意思就是函数声明存在变量提升(英文一样是hoisted)，这里是怕先调用再声明，影响代码可维护性

``` JavaScript
      // bad
      function foo() {
        // ...
      }

      // bad
      const foo = function () {
        // ...
      };

      // good
      // lexical name distinguished from the variable-referenced invocation(s)
      const short = function longUniqueMoreDescriptiveLexicalFoo() {
        // ...
      };
```

* 使用立即调用函数表达式（[IIFE](https://eslint.org/docs/rules/wrap-iife.html)）

```JavaScript
      // immediately-invoked function expression (IIFE) 立即调用的函数表达式
      (function () {
        console.log('Welcome to the Internet. Please follow me.');
      }());
```

* 切记不要在非功能块中声明函数 (if, while, 等)。 将函数赋值给变量。 [eslint](https://eslint.org/docs/rules/no-loop-func.html)

```JavaScript
      // 这样数组funcs中都返回10
      for (var i = 0; i < 10; i++) {
          funcs[i] = function() {
              return i;
          };
      }
      // 可以改为用let
      for (let i = 0; i < 10; i++) {
          funcs[i] = function() {
              return i;
          };
      }
      // 也可以改为用立即执行（注意要向内传参数）
      for (var i = 0; i < 10; i++) {
          (function(i){funcs[i] = function() {
              return i;
          };}(i))
      }
```

* 不要使用 arguments, 选择使用 rest 语法 ... 代替

    为什么? ... 明确了你想要拉取什么参数。 更甚, rest 参数是一个真正的数组，而不仅仅是类数组的 arguments 。

```JavaScript
      // bad
      function concatenateAll() {
        const args = Array.prototype.slice.call(arguments);
        return args.join('');
      }

      // good
      function concatenateAll(...args) {
        return args.join('');
      }
```

* 总是把默认参数放在最后。

```JavaScript
      // bad
      function handleThings(opts = {}, name) {
        // ...
      }

      // good
      function handleThings(name, opts = {}) {
        // ...
      }
```

* 函数签名中的间距。

  为什么? 一致性很好，在删除或添加名称时不需要添加或删除空格。

```JavaScript
      // bad
      const f = function(){};
      const g = function (){};
      const h = function() {};

      // good
      const x = function () {};
      const y = function a() {};
```

* 具有多行签名或者调用的函数应该像本指南中的其他多行列表一样缩进：在一行上只有一个条目，并且每个条目最后加上逗号。 

```JavaScript
      // bad
      function foo(bar,
                  baz,
                  quux) {
        // ...
      }

      // good
      function foo(
        bar,
        baz,
        quux,
      ) {
        // ...
      }

      // bad
      console.log(foo,
        bar,
        baz);

      // good
      console.log(
        foo,
        bar,
        baz,
      );
```

* 如果函数体包含一个单独的语句，返回一个没有副作用的 expression ， 省略括号并使用隐式返回。否则，保留括号并使用 return 语句

* 如果表达式跨越多个行，用括号将其括起来，以获得更好的可读性。

## 类和构造器

* 尽量使用 class. 避免直接操作 prototype

```JavaScript
      // bad
      function Queue(contents = []) {
        this.queue = [...contents];
      }
      Queue.prototype.pop = function () {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      };

      // good
      class Queue {
        constructor(contents = []) {
          this.queue = [...contents];
        }
        pop() {
          const value = this.queue[0];
          this.queue.splice(0, 1);
          return value;
        }
      }
```

* 使用 extends 来扩展继承。

    为什么? 它是一个内置的方法，可以在不破坏 instanceof 的情况下继承原型功能。

```JavaScript
      // bad
      const inherits = require('inherits');
      function PeekableQueue(contents) {
        Queue.apply(this, contents);
      }
      inherits(PeekableQueue, Queue);
      PeekableQueue.prototype.peek = function () {
        return this.queue[0];
      };

      // good
      class PeekableQueue extends Queue {
        peek() {
          return this.queue[0];
        }
      }
```

* 方法返回了 this 来供其内部方法调用

```JavaScript
      // bad
      Jedi.prototype.jump = function () {
        this.jumping = true;
        return true;
      };

      Jedi.prototype.setHeight = function (height) {
        this.height = height;
      };

      const luke = new Jedi();
      luke.jump(); // => true
      luke.setHeight(20); // => undefined

      // good
      class Jedi {
        jump() {
          this.jumping = true;
          return this;
        }

        setHeight(height) {
          this.height = height;
          return this;
        }
      }

      const luke = new Jedi();

      luke.jump()
        .setHeight(20);
```

* 如果没有指定类，则类具有默认的构造器。 不用写空构造器

```JavaScript
      // bad
      class Jedi {
        constructor() {}

        getName() {
          return this.name;
        }
      }

      // bad
      class Rey extends Jedi {
        constructor(...args) {
          super(...args);
        }
      }

      // good
      class Rey extends Jedi {
        constructor(...args) {
          super(...args);
          this.name = 'Rey';
        }
      }
```

## 模块

* 尽量使用es的import而非require

* 只从一个路径导入所有需要的东西

```JavaScript
      // bad
      import foo from 'foo';
      // … 其他导入 … //
      import { named1, named2 } from 'foo';

      // good
      import foo, { named1, named2 } from 'foo';

      // good
      import foo, {
        named1,
        named2,
      } from 'foo';
```

* 不导出可变的引用

```JavaScript
      // bad
      let foo = 3;
      export { foo };

      // good
      const foo = 3;
      export { foo };
```
* 单个的导出模块，鼓励使用default导出

  * export default与export的主要区别是  不需要知道导出的具体变量名  与  导入时不需要{}
  ```JavaScript
      //export default
      export default function crc32(){};
      import crc32 from 'crc32';

      //export
      export function crc32() {};
      import {crc32} from 'crc32';
  ```

  * export原理如下
  ```JavaScript
      var name = 'Mark';
      export default name;  => export {name as default};
      import surname from '...'; => import {default as surname} from '..';
  ```

  * 一个模块中只能有一个export default默认输出

* 多行导入应该像多行数组和对象一样缩进

  ```JavaScript
      // bad
      import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

      // good
      import {
        longNameA,
        longNameB,
        longNameC,
        longNameD,
        longNameE,
      } from 'path';
  ```

## 属性

计算指数时，可以使用 ** 运算符

```JavaScript
// bad
const binary = Math.pow(2, 10);

// good
const binary = 2 ** 10;
```

## 变量

* 声明多个变量时，每个都用const或者let修饰符，不要用逗号

* 对于布尔值使用简写，但是对于字符串和数字进行显式比较。

```JavaScript
      // bad
      if (isValid === true) {
        // ...
      }

      // good
      if (isValid) {
        // ...
      }

      // bad
      if (name) {
        // ...
      }

      // good
      if (name !== '') {
        // ...
      }

      // bad
      if (collection.length) {
        // ...
      }

      // good
      if (collection.length > 0) {
        // ...
      }
```

* 在 case 和 default 的子句中，如果存在声明 (例如. let, const, function, 和 class)，使用大括号来创建块

```JavaScript
      // bad
      switch (foo) {
        case 1:
          let x = 1;
          break;
        case 2:
          const y = 2;
          break;
        case 3:
          function f() {
            // ...
          }
          break;
        default:
          class C {}
      }

      // good
      switch (foo) {
        case 1: {
          let x = 1;
          break;
        }
        case 2: {
          const y = 2;
          break;
        }
        case 3: {
          function f() {
            // ...
          }
          break;
        }
        case 4:
          bar();
          break;
        default: {
          class C {}
        }
      }
```

* 避免不必要的三目表达式

## 控制语句

* 使用 // FIXME: 注释一个问题。  

* 使用 // TODO: 注释解决问题的方法。

## 空格

* 使用 tabs (空格字符) 设置为 2 个空格。

* 在块和下一个语句之前留下一空白行。

* 不要在块的开头使用空白行

* 不要在括号内添加空格

```JavaScript
      // bad
      function bar( foo ) {
        return foo;
      }

      // good
      function bar(foo) {
        return foo;
      }

      // bad
      if ( foo ) {
        console.log(foo);
      }

      // good
      if (foo) {
        console.log(foo);
      }
```

## 逗号

对象中最后一行也加上逗号

```JavaScript
      // bad - 没有尾随逗号的 git 差异
      const hero = {
          firstName: 'Florence',
      -    lastName: 'Nightingale'
      +    lastName: 'Nightingale',
      +    inventorOf: ['coxcomb chart', 'modern nursing']
      };

      // good - 有尾随逗号的 git 差异
      const hero = {
          firstName: 'Florence',
          lastName: 'Nightingale',
      +    inventorOf: ['coxcomb chart', 'modern nursing'],
      };
```
