## const
------------------------

简单类型数据常量

```
// const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。
const PI = 3.1415926;
console.log(PI)
```

对象常量

```
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```

除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数。

```
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

##属性的简洁表示
------------------------

对象，函数都可以简写

```
var birth = '2000/01/01';
var Person = {
  name: '张三',
  //等同于birth: birth
  birth,
  // 等同于hello: function ()...
  hello() { console.log('我的名字是', this.name); }
};
```


CommonJS模块输出变量，就非常合适使用简洁写法。

```
var ms = {};

function getItem(key) {
  return key in ms ? ms[key] : null;
}

function setItem(key, value) {
  ms[key] = value;
}

function clear() {
  ms = {};
}

module.exports = {
  getItem,
  setItem,
  clear
};
// 等同于
module.exports = {
  getItem: getItem,
  setItem: setItem,
  clear: clear
};
```


##Object.is()
------------------------

```
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

##Object.assign()
------------------------

用于对象的合并，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

```
var target = { a: 1 };

var source1 = { b: 2 };
var source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

注意点：
Object.assign 方法实行的是浅拷贝

```
var obj1 = {a: {b: 1}};
var obj2 = Object.assign({}, obj1);

obj1.a.b = 2;
obj2.a.b // 2
```

###常见用途：
（1）为对象添加属性

```
class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
```

（2）为对象添加方法

```
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {
    ···
  },
  anotherMethod() {
    ···
  }
});

// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
};
SomeClass.prototype.anotherMethod = function () {
  ···
};
```

（3）克隆对象
```
function clone(origin) {
  return Object.assign({}, origin);
}
```
不过，采用这种方法克隆，只能克隆原始对象自身的值，不能克隆它继承的值。

想要保持继承链，可以采用下面的代码。
```
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
}
```


（4）合并多个对象
```
const merge =
  (target, ...sources) => Object.assign(target, ...sources);
const merge =
  (...sources) => Object.assign({}, ...sources);
```

（5）为属性指定默认值
```
const DEFAULTS = {
  logLevel: 0,
  outputFormat: 'html'
};

function processContent(options) {
  options = Object.assign({}, DEFAULTS, options);
  console.log(options);
  // ...
}
```
由于存在深拷贝的问题，DEFAULTS对象和options对象的所有属性的值，最好都是简单类型，不要指向另一个对象。



##属性的遍历
------------------------
ES6一共有5种方法可以遍历对象的属性。

###（1）for...in

for...in循环遍历对象自身的和继承的可枚举属性（不含Symbol属性）。

###（2）Object.keys(obj)

Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）。

###（3）Object.getOwnPropertyNames(obj)

Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含Symbol属性，但是包括不可枚举属性）。

###（4）Object.getOwnPropertySymbols(obj)

Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有Symbol属性。

###（5）Reflect.ownKeys(obj)

Reflect.ownKeys返回一个数组，包含对象自身的所有属性，不管是属性名是Symbol或字符串，也不管是否可枚举。



##Object.keys()，Object.values()，Object.entries()
ES5 引入了Object.keys方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。
```
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj)
// ["foo", "baz"]
```
ES2017 引入了跟Object.keys配套的Object.values和Object.entries，作为遍历一个对象的补充手段，供for...of循环使用。
```
var obj = { foo: 'bar', baz: 42 };
Object.values(obj)
// ["bar", 42]
```

##对象的扩展运算符
------------------------
###（1）解构赋值
```
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```
###（2）扩展运算符
```
let z = { a: 3, b: 4 };
let n = { ...z };
n // { a: 3, b: 4 }
```

##Null 传导运算符
------------------------
编程实务中，如果读取对象内部的某个属性，往往需要判断一下该对象是否存在。比如，要读取message.body.user.firstName，安全的写法是写成下面这样。
```
const firstName = (message
  && message.body
  && message.body.user
  && message.body.user.firstName) || 'default';
```
这样的层层判断非常麻烦，因此现在有一个提案，引入了“Null 传导运算符”（null propagation operator）?.，简化上面的写法。
```
const firstName = message?.body?.user?.firstName || 'default';
```
