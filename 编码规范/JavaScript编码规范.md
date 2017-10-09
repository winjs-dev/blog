# JavaScript编码规范

----------

<a name='TOC'>内容列表</a>

- [类型](#types)
- [对象](#objects)
- [字符串](#strings)
- [函数](#functions)
- [属性](#properties)
- [变量](#variables)
- [条件表达式及等号](#condtionals)
- [注释](#comments)
- [逗号](#commas)
- [分号](#semicolons)
- [大括号](#blocks)
- [圆括号](#parentheses)
- [类型转换](#type-coercion)
- [命名约定](#naming-conventions)
- [存取器](#accessors)
- [事件](#events)
- [jQuery](#jquery)
- [文件编码](#codedFormat)

## <a name='types'>类型</a>

- **原始值**: 相当于传值
+ `string`
+ `number`
+ `boolean`
+ `null`
+ `undefined`

```
var foo = 1,
 bar = foo;

 bar = 9;

 console.log(foo, bar); // => 1,9
```

- **复杂类型**: 相当于传引用

+ `object`
+ `array`
+ `function`

```
var foo = [1, 2],
  bar = foo;

  bar[0] = 9;

  console.log(foo[0], bar[0]); // => 9,9
    
```    


## <a name='objects'>对象</a>

- 使用字面值创建对象

```
// bad
var item = new Object();

// good
// var item = {};
```


- 不要使用保留字 [reserved words](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words) 作为键

```
// bad
var superman = {
class: 'superhero',
default: { clark: 'kent' },
private: true
};

// good
var superman = {
klass: 'superhero',
defaults: { clark: 'kent' },
hidden: true
};
```

## <a name='strings'>字符串</a>

- 对字符串使用单引号 `''`

```
// bad
var name = "Bob Parr";

// good
var name = 'Bob Parr';

// bad
var fullName = "Bob " + this.lastName;

// good
var fullName = 'Bob ' + this.lastName;
```

- 超过80个字符的字符串应该使用字符串连接换行
- 注: 如果过度使用，长字符串连接可能会对性能有影响

```
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
var errorMessage = 'This is a super long error that \
  was thrown because of Batman. \
  When you stop to think about \
  how Batman had anything to do \
  with this, you would get nowhere \
  fast.';

// good
var errorMessage = 'This is a super long error that ' +
'was thrown because of Batman.' +
'When you stop to think about ' +
'how Batman had anything to do ' +
'with this, you would get nowhere ' +
'fast.';
```

- 编程时使用join而不是字符串连接来构建字符串，特别是IE:

```
var items,
  messages,
  length, i;

messages = [{
  state: 'success',
  message: 'This one worked.'
},{
  state: 'success',
  message: 'This one worked as well.'
},{
  state: 'error',
  message: 'This one did not work.'
}];

length = messages.length;

// bad
function inbox(messages) {
items = '<ul>';

for (i = 0; i < length; i++) {
  items += '<li>' + messages[i].message + '</li>';
}

return items + '</ul>';
}

// good
function inbox(messages) {
items = [];

for (i = 0; i < length; i++) {
  items[i] = messages[i].message;
}

return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
}
```

## <a name='functions'>函数</a>

- 函数表达式:

```
// 匿名函数表达式
var anonymous = function() {
  return true;
};

// 有名函数表达式
var named = function named() {
  return true;
};

// 立即调用函数表达式
(function() {
  console.log('Welcome to the Internet. Please follow me.');
})();
```

- 绝对不要在一个非函数块里声明一个函数，把那个函数赋给一个变量。浏览器允许你这么做，但是它们解析不同。
- **注:** ECMA-262定义把`块`定义为一组语句，函数声明不是一个语句。[阅读ECMA-262对这个问题的说明](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

```
// bad
if (currentUser) {
function test() {
  console.log('Nope.');
}
}

// good
if (currentUser) {
var test = function test() {
  console.log('Yup.');
};
}
```

- 绝对不要把参数命名为 `arguments`, 这将会逾越函数作用域内传过来的 `arguments` 对象.

```
// bad
function nope(name, options, arguments) {
// ...stuff...
}

// good
function yup(name, options, args) {
// ...stuff...
}
```

**注：所有函数都在使用之前定义。**

**[[⬆]](#TOC)**


## <a name='properties'>属性</a>

- 当使用变量访问属性时使用中括号.

```
var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
```


## <a name='variables'>变量</a>

- 总是使用 `var` 来声明变量，如果不这么做将导致产生全局变量，我们要避免污染全局命名空间。

```
// bad
superPower = new SuperPower();

// good
var superPower = new SuperPower();
```

- 使用一个 `var` 以及新行声明多个变量，缩进4个空格。

```
// bad
var items = getItems();
var goSportsTeam = true;
var dragonball = 'z';

// good
var items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';
```

- 最后再声明未赋值的变量，当你想引用之前已赋值变量的时候很有用。

```
// bad
var i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
var i, items = getItems(),
    dragonball,
    goSportsTeam = true,
    len;

// good
var items = getItems(),
    goSportsTeam = true,
    dragonball,
    length,
    i;
```

- 在作用域顶部声明变量，避免变量声明和赋值引起的相关问题。因此，所有变量声明都放在函数的头部。

```
// bad
function() {
  test();
  console.log('doing stuff..');

  //..other stuff..

  var name = getName();

  if (name === 'test') {
    return false;
  }

  return name;
}

// good
function() {
  var name = getName();

  test();
  console.log('doing stuff..');

  //..other stuff..

  if (name === 'test') {
    return false;
  }

  return name;
}

// bad
function() {
  var name = getName();

  if (!arguments.length) {
    return false;
  }

  return true;
}

// good
function() {
  if (!arguments.length) {
    return false;
  }

  var name = getName();

  return true;
}
```

**[[⬆]](#TOC)**


## <a name='conditionals'>条件表达式和等号</a>

- 适当使用 `===` 和 `!==` 以及 `==` 和 `!=`.
- 条件表达式的强制类型转换遵循以下规则：

+ **对象** 被计算为 **true**
+ **Undefined** 被计算为 **false**
+ **Null** 被计算为 **false**
+ **布尔值** 被计算为 **布尔的值**
+ **数字** 如果是 **+0, -0, or NaN** 被计算为 **false** , 否则为 **true**
+ **字符串** 如果是空字符串 `''` 则被计算为 **false**, 否则为 **true**

```
if ([0]) {
  // true
  // An array is an object, objects evaluate to true
}
```

- 使用快捷方式.

```
// bad
if (name !== '') {
  // ...stuff...
}

// good
if (name) {
  // ...stuff...
}

// bad
if (collection.length > 0) {
  // ...stuff...
}

// good
if (collection.length) {
  // ...stuff...
}
```

- 阅读 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) 了解更多


## <a name='comments'>注释</a>

- 使用 `/** ... */` 进行多行注释，包括描述，指定类型以及参数值和返回值

```
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param <String> tag
// @return <Element> element
function make(tag) {

  // ...stuff...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param <String> tag
 * @return <Element> element
 */
function make(tag) {

  // ...stuff...

  return element;
}
```

- 使用 `//` 进行单行注释，在评论对象的上面进行单行注释，注释前放一个空行.

```
// bad
var active = true;  // is current tab

// good
// is current tab
var active = true;

// bad
function getType() {
  console.log('fetching type...');
  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}

// good
function getType() {
  console.log('fetching type...');

  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}
```


## <a name='commas'>逗号</a>

- 不要将逗号放前面

```
// bad
var once
  , upon
  , aTime;

// good
var once,
    upon,
    aTime;

// bad
var hero = {
    firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};

// good
var hero = {
  firstName: 'Bob',
  lastName: 'Parr',
  heroName: 'Mr. Incredible',
  superPower: 'strength'
};
```

- 不要加多余的逗号，这可能会在IE下引起错误，同时如果多一个逗号某些ES3的实现会计算多数组的长度。

```
// bad
var hero = {
  firstName: 'Kevin',
  lastName: 'Flynn',
};

var heroes = [
  'Batman',
  'Superman',
];

// good
var hero = {
  firstName: 'Kevin',
  lastName: 'Flynn'
};

var heroes = [
  'Batman',
  'Superman'
];
```


## <a name='semicolons'>分号</a>

- 语句结束一定要加分号

```
// bad
(function() {
  var name = 'Skywalker'
  return name
})()

// good
(function() {
  var name = 'Skywalker';
  return name;
})();

// good
;(function() {
  var name = 'Skywalker';
  return name;
})();
```


## <a name='blocks'>大括号</a>

- 给所有多行的块使用大括号

```
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function() { return false; }

// good
function() {
  return false;
}

```
- 大括号的位置，绝大多数的编程语言，都用大括号（{}）表示区块（block）。起首的大括号的位置，有许多不同的写法。
最流行的有两种。一种是起首的大括号另起一行：

 ```
block
　　{
　　　　...
　　}
  ```

另一种是起首的大括号跟在关键字的后面：

 ```
block {
　　　　...
}
```

一般来说，这两种写法都可以接受。但是，Javascript要使用后一种，因为Javascript会自动添加句末的分号，导致一些难以察觉的错误。

```
　return
　　{
　　　　key:value;
　　};
```
上面的代码的原意，是要返回一个对象，但实际上返回的是undefined，因为Javascript自动在return语句后面添加了分号。为了避免这一类错误，需要写成下面这样：

```
return {
　　　　key : value;
　　};
```
因此，**表示区块起首的大括号，不要另起一行。**


## <a name='parentheses'>圆括号</a>

- 圆括号，在Javascript中有两种作用，一种表示调用函数，另一种表示不同的值的组合（grouping）。我们可以用空格，区分这两种不同的括号。

**调用函数的时候，函数名与左括号之间没有空格。**

```
function add() {
　　　　
　　}
```
**函数名与参数序列之间，没有空格。**

```
function add(parame1, parame2) {
　　　　
　　}
```
**所有其他语法元素与左括号之间，都有一个空格。**
```
width (o) {
    foo = bar;
}
```


## <a name='type-coercion'>类型转换</a>

- 在语句的开始执行类型转换.
- 字符串:

```
//  => this.reviewScore = 9;

// bad
var totalScore = this.reviewScore + '';

// good
var totalScore = '' + this.reviewScore;

// bad
var totalScore = '' + this.reviewScore + ' total score';

// good
var totalScore = this.reviewScore + ' total score';
```

- 对数字使用 `parseInt` 并且总是带上类型转换的基数.

```
var inputValue = '4';

// bad
var val = new Number(inputValue);

// bad
var val = +inputValue;

// bad
var val = inputValue >> 0;

// bad
var val = parseInt(inputValue);

// good
var val = Number(inputValue);

// good
var val = parseInt(inputValue, 10);

// good
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
var val = inputValue >> 0;
```

- 布尔值:

```
var age = 0;

// bad
var hasAge = new Boolean(age);

// good
var hasAge = Boolean(age);

// good
var hasAge = !!age;
```


## <a name='naming-conventions'>命名约定</a>

- 避免单个字符名，让你的变量名有描述意义。

```
// bad
function q() {
  // ...stuff...
}

// good
function query() {
  // ..stuff..
}
```

- 当命名对象、函数和实例时使用驼峰命名规则

```
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
var this-is-my-object = {};
function c() {};
var u = new user({
  name: 'Bob Parr'
});

// good
var thisIsMyObject = {};
function thisIsMyFunction() {};
var user = new User({
  name: 'Bob Parr'
});
```

- 当命名构造函数或类时使用驼峰式大写

```
// bad
function user(options) {
  this.name = options.name;
}

var bad = new user({
  name: 'nope'
});

// good
function User(options) {
  this.name = options.name;
}

var good = new User({
  name: 'yup'
});
```

- 命名私有属性时前面加个下划线 `_`

```
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```

- 当保存对 `this` 的引用时使用 `_this`.

```
// bad
function() {
  var self = this;
  return function() {
    console.log(self);
  };
}

// bad
function() {
  var that = this;
  return function() {
    console.log(that);
  };
}

// good
function() {
  var _this = this;
  return function() {
    console.log(_this);
  };
}
```

## <a name='accessors'>存取器</a>

- 属性的存取器函数不是必需的
- 如果你确实有存取器函数的话使用getVal() 和 setVal('hello')

```
// bad
dragon.age();

// good
dragon.getAge();

// bad
dragon.age(25);

// good
dragon.setAge(25);
```

- 如果属性是布尔值，使用isVal() 或 hasVal()

```
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

- 可以创建get()和set()函数，但是要保持一致

```
function Jedi(options) {
  options || (options = {});
  var lightsaber = options.lightsaber || 'blue';
  this.set('lightsaber', lightsaber);
}

Jedi.prototype.set = function(key, val) {
  this[key] = val;
};

Jedi.prototype.get = function(key) {
  return this[key];
};
```

## <a name='events'>事件</a>

- 当给事件附加数据时，传入一个哈希而不是原始值，这可以让后面的贡献者加入更多数据到事件数据里而不用找出并更新那个事件的事件处理器

```
// bad
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', function(e, listingId) {
  // do something with listingId
});
```

更好:

```
// good
$(this).trigger('listingUpdated', { listingId : listing.id });

...

$(this).on('listingUpdated', function(e, data) {
  // do something with data.listingId
});
```

## <a name='jquery'>jQuery</a>

- 缓存jQuery查询

```
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...stuff...

  $('.sidebar').css({
    'background-color': 'pink'
  });
}

// good
function setSidebar() {
  var $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...stuff...

  $sidebar.css({
    'background-color': 'pink'
  });
}
```

- 对DOM查询使用级联的 `$('.sidebar ul')` 或 `$('.sidebar ul')`
- 对有作用域的jQuery对象查询使用 `find`

```
// bad
$('.sidebar', 'ul').hide();

// bad
$('.sidebar').find('ul').hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good (slower)
$sidebar.find('ul');

// good (faster)
$($sidebar[0]).find('ul');
```

## <a name='codedFormat'>文件编码</a>

- JavaScript 文件使用无 BOM 的 UTF-8 编码。UTF-8 编码具有更广泛的适应性。BOM 在使用程序或工具处理文件时可能造成不必要的干扰。

