# 变量

### 变量命名

变量包括普通变量、成员变量、类变量、函数等。

变量命名尽量使用浅显易懂的英文单词组合而成，较长单词可使用缩写，对于不容易理解的变量名应多写注释。

一般变量使用 **驼峰** 形式命名，常量使用全大写形式，并在单词之间用`_`隔开。

类名也采用驼峰形式，且首字母大写。

```javascript
// 常量
const GAME_NAME = 'dn';

// 普通变量
const playerName = 'liming';

// 函数
function playGame() {
    console.log(`${playerName} is playing ${gameName}`);
}

// 类
class Person {}
```

### 变量定义

变量定义包括声明（自动初始化）和赋值两步。

ES6引入了两个新的变量声明相关的关键字`const`和`let`，实现了真正的块级作用域。

对于 **不可修改** 变量必须使用`const`进行声明，**不会修改** 变量尽量使用`const`声明。

可修改变量使用`let`声明。

变量声明和赋值尽量一步搞定，并一行定义一个变量，即使是最简单的`let i = 0;`。

**禁止使用链式赋值方式，该方式存在隐蔽的全局变量问题。**

```javascript
// bad
(function example() {
  // JavaScript interprets this as
  // let a = ( b = ( c = 1 ) );
  // The let keyword only applies to variable a; variables b and c become
  // global variables.
  let a = b = c = 1;
}());

console.log(a); // undefined
console.log(b); // 1
console.log(c); // 1
```

**使用const声明的变量必须在初始化的时候赋值，因为const变量不能进行二次赋值。**

```javascript
// bad
const a;
a = 1; // Uncaught SyntaxError: Missing initializer in const declaration

// good
const b = 2;
```

**禁止使用~~var~~，因为存在变量提升、变量重复声明、变量覆盖等隐蔽的棘手问题。**

>虽然let和const也存在变量提升，不过此时变量存在时间死区（[TDZ](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified/)）里面，并没有初始化，任何访问都会报错，而var会自动将变量初始化为undefined。

而且，let和const都是块级作用域的，可以减少作用域冲突问题。

在变量需要使用时才定义。

```javascript
function enterRoom(id) {
    if(!id)
        return;

    // 前面的代码根本不需要opts
    const opts = createOpts();
    _enter(id, opts);
}
```

数组和对象类型的变量，在创建的时候直接使用字面量（[], {}）进行创建。

如果一个对象只需要当做简单的哈希类型来使用，可以使用`Object.create(null);`来进行创建纯净的对象变量。

```javascript
const arr = [1, 2, 3];
const obj = {a: 1, b: 2};
const map = Object.create(null);
```

### 全局变量

尽量避免对全局变量进行增删改，需要跨文件共享变量请使用模块化功能。

```javascript
// a.js
export const name = 'dn'; // 跨文件共享name变量

// b.js
import {name} from './a.js';
assert(name, 'dn');
```

# 格式相关

### 缩进

严格使用 **空格** 进行缩进，一个tab设置为4个空格。

禁止混用空格和tab。

### 单行长度

传统建议一行不超过80字符，具体可视显示器而定。

### 空格

情况较多，可借助格式化工具进行规范。

### 分号

表达式行尾必须加分号。

`if`、`for`、`while`等语句末和`class`、`function`声明等行尾不加。

类和函数以赋值方式定义的需要加。

```javascript
const fn = function() {}; // 注意分号
function fn() {}
const Game = class {}; // 注意分号
class Game {}
```

### 花括号

左花括号跟在行尾，右花括号另起一行，参考前面的代码。

`if`、`else` 里面如果只有一行语句，不用再写花括号了，为了格式好看，换行和缩进还是需要，这种情况也可以使用三元表达式。

如果只有if，可以使用`&&`或`||`进行简写，有`return`的除外。

逻辑运算都是短路的。

```javascript
if(true)
    doSomething();

isTrue ? fn1() : fn2();

isTrue && fn1();
```

`switch`的`case`和`default`语句使用花括号。

以上非功能性语法格式大多可以借助IDE或者`jsFormat`等插件进行格式化，达到规范的效果。

# 语法相关

### 严格模式

~~总是使用严格模式，文件第一行必须是`'use strict;'`。~~

使用ES6模块化语法和`class`自动开启严格模式。

严格模式会限制很多出乎意料的情况，避免在运行时才发现，如全局变量、对象属性重复等。

### 字符串

简单字符串变量使用 **单引号** ，内容含有单引号的字符串使用双引号，单双引号都有的字符串使用反引号`\``，尽量避免字符串里面使用转义符。

反引号是ES6引入的新语法，是一种模板字符串，可以内嵌变量，最常用的就是用来取代字符串拼接。

```javascript
const world = 'world';

const str1 = 'hello, world';
const str2 = "hello', world";
const str3 = `'hello', "world"`;
const str4 = `hello ${world}`;
```

### 数组、对象

数组和对象的最后一个元素后面不能留逗号。

对象的key如果是一个合法的变量名就不加引号，否则加单引号。（高级压缩需要保留除外）

```javascript
const arr = [1, 2, 3/* 这里不要留逗号*/];
const obj = {
    a: 1,
    // bad
    'b': 2,
    // good
    // 尽量避免这种key
    'c-d': 3 // 这里不要留逗号，在一些较早的引擎里会出错
}
```

对象的属性获取使用 **`.`** ，如果属性名不是一个合法的变量名，才使用`[]`。（高级压缩需要保留除外）

### 函数

因为函数声明也存在变量提升的问题，应尽量使用赋值的方式定义函数，同时为函数命名会更方便查问题。

不过，如果是在模块里面，function也不会提升到其他模块去了，使用函数声明的影响也不算大。

```javascript
const playGame = function playGame() {
};

function playGame() {
}
```

需要注意的是，只有使用变量名才能进行函数调用，使用函数名是不行的，实际上函数名经过提升了，不过处于TDZ，访问就会报错。

```javascript
const playGame = function _playGame() {
};

console.log(playGame.name); // _playGame

_playGame(); // Uncaught ReferenceError: _playGame is not defined
```

避免在`if`、`while`等语句里面定义函数，不过可以进行函数赋值。

```javascript
let fn;
if(isTrue) {
    fn = function () {};
    // or
    fn = () => {};
}
fn && fn();
```

函数尽早return，减少缩进，条理清晰。

```javascript
function enterRoom(id) {
    if(!id)
        return;
    if(!isNumber(id))
        return;
    if(!isValid(id))
        return;
    // enterRoom
}
```

### null和undefined

`null`和`undefined`都是一种空值（通过布尔类型转换!!为false的值），两者容易混用，需要注意两者各自的使用场景。

null表示没有值，通常用在不应有值的地方，意思就是不用怀疑，此处就是不应该有值，如果有值那就是出错了。

举个栗子，比如进行一次网络请求，如果结果成功了，传给回调函数的第一个参数应该为null，回调函数认为该参数是一个error，如果接收到null，那就代表没有error，就是成功了。

```javascript
function callback(err, data) {

}
function request(callback) {
    // if success
    return callback(null, data);
    // if fail
    callback(new Error('request error'));
}

function requestPromise() {
    return new Promise(resolve => {
        // if success
        return resolve([null, data]);
        // if fail
        resolve([new Error('request error')]);
    });
}
```

再如`Object.create(null);`，传null表示创建出来的对象不应该有原型。

而undefined表示一个未定义的值，意思是此处其实有值，但是未定义，通常不需要关心其具体内容。

比如用在函数返回值、变量声明而未赋值、访问不存在的对象属性等情况。

### ++/--

避免在for循环以外使用++/--，使用+=/-=代替。

### 比较操作

使用`===`、`!==`，而不是`==`、`!=`。

`if`判断的表达式会自动转换成布尔类型，无需显式做一次判断。

使用`!`和`!!`将变量转换成布尔变量。

当需要判断`+0`和`-0`，以及`NaN`时，比较操作符会出乎意料，这时候应使用`Object.is`。

```javascript
+0 === -0; // true
Object.is(+0, -0); // false
NaN === NaN; // false
Object.is(NaN, NaN); // true
```

# ES6新语法

>以下规范全面使用ES2015（旧称ES6）新语法，需要引擎支持，或者使用babel等工具进行转换。

>完整的新语法教程推荐阅读[understandinges6](https://github.com/nzakas/understandinges6)。

部分规范参考[Airbnb的js规范](https://github.com/airbnb/javascript)。

### let和const

一句话，不会修改变量使用const，会修改变量使用let，禁用var。

### export和import

ES6终于在官方层面支持了JS的模块化语法。

>推荐阅读[Modules](https://github.com/nzakas/understandinges6/blob/master/manuscript/13-Modules.md)。

### 箭头函数

箭头函数和普通函数并无太大差别，最大的好处在于解决了函数在运行时存在this不确定的问题。

箭头函数在声明的时候就确定了this，而且不会在运行时改变。

>this对于初学js的人就是个天坑，推荐阅读[理解this](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)。

多使用箭头函数总是好的，特别是一些使用短函数（lambda）的场景。

```javascript
const arr = [1, 2, 3].map(number => number * number);
```

当箭头函数只有一个参数时，可省略圆括号；
当函数体只有一行语句时，可以省略花括号，连`return`也可以省略。

**箭头函数不能使用关键字`new`进行调用，因为箭头函数没有`[[Construct]]`方法。**

箭头函数行为比较单一，不能改变this、不能使用new进行调用，所以JS引擎一般也好优化。

### class

需要类的地方使用`class`关键字进行声明，注意是小写，使用`extends`实现继承，不推荐继续使用原型链。

>完整的class教程[Classes](https://github.com/nzakas/understandinges6/blob/master/manuscript/09-Classes.md)。

类成员函数只能使用简写语法， **成员之间没有逗号**。

类属性需在`constructor`方法内部进行声明和初始化。

静态成员在前加`static`关键字，目前只支持方法，不支持属性。

```javascript
class DN {
    play() {
        console.log('enjoy!');
    } // 区别于对象，这里不要写逗号
    // 暂不支持
    // static name = 'hldn';
    static get name() {
        return 'hldn';
    }
}
```

**在constructor方法里面使用this关键字之前，需要先调用`super();`。**

```javascript
class Player {
    constructor(id, name) {
        this.id = id;
        this.name = name;
    }

    play() {
        console.log(`${this.name} is playing game`);
    }
}

class DNPlayer extends Player {
    constructor(...args) {
        super(...args); // 一定要在使用this之前调用
        this.tag = 'dn'; // 否则报错 Uncaught ReferenceError: this is not defined
    }
}
```

### 对象

和class类似，对象的函数属性也推荐使用简写语法。

其他属性当属性名和变量名一样的时候也使用简写，其实这是一种很常见的情况。

一句话，能简则简。

```javascript
const name = 'dn';

const DN = {
    // 等价于 name: name,
    name,

    // 等价于 play: function() {console.log('enjoy!');}
    play() {
        console.log('enjoy!');
    }
};
```

另外，为了排版上好看，将所有简写的属性放在对象的开头，或者放在一起。

当属性名需要动态计算的时候，使用`computed property names`。

`computed property names`可以直接在对象字面量里使用。

```javascript
function getKey() {
    return String(Date.now());
}
const obj = {
    // computed property name
    // 方括号加上表达式
    [getKey()]: 'the key is a timestamp'
};
```

使用`...`语法复制或者合并对象。

```javascript
const obj1 = {a: 1, b: 2};
const obj2 = {b: 3, c: 4};

const obj3 = {...obj1};
// {a: 1, b: 2}
// 花括号不能忘了，数组的话不能忘了方括号
const obj4 = {...obj1, ...obj2};
// {a: 1, b: 3, c: 4}
```

数组也可以使用类似的语法。

### 解构赋值

解构赋值是ES6增加的一个非常有用的新特性，可以看做是对象简写的逆过程。

>详细教程[Destructuring](https://github.com/nzakas/understandinges6/blob/master/manuscript/05-Destructuring.md)。

```javascript
const DN = {
    name,
    play() {
        console.log('enjoy!');
    }
};
const games = ['dn', 'ddz'];

// 相当于
// const name = DN.name;
// const play = DN.play;
const {name, play} = DN;

// 重命名
// const playGame = DN.play;
const {name, play: playGame} = DN;

// 使用默认值
// 当且仅当name不存在或者为undefined的时候才会被赋值为'hldn'
const {name = 'hldn', play} = DN;

// 数组也一样
const [game1, game2] = games;

const [err, data] = await requestPromise();
```

另外，再看看如何轻松交换两个变量的值。

```javascript
let a = 1;
let b = 2;

console.log(a, b); // 1, 2

// 和数组的解构赋值很像，但并不是一样的
[a, b] = [b, a];

console.log(a, b); // 2, 1
```

### 默认参数

当函数参数需要默认值的时候，使用新的默认参数语法。

```javascript
function enterRoom(id = defaultID) {
    _enter(id);
}
```

**只有id是undefined的时候默认参数才会生效，其他空值（null、0等）并不会生效。**

**undefined可以是不传参数或者显式传`undefined`。**

默认参数可以在函数参数的任何位置，并不一定要求在最后，不过如果不是末尾，在调用的时候要格外小心。

默认参数可以是变量、表达式，甚至是前面的形参，参见[Functions](https://github.com/nzakas/understandinges6/blob/master/manuscript/03-Functions.md)。

### 剩余参数

使用剩余参数替代`arguments`，并且不要再使用arguments了。

```javascript
function enterRoom(id, ...args) {
    // args是一个数组
    _enter(id)(...args);
}
```

**剩余参数也是`...`语法，并且只能出现在函数参数的最后面，且只能使用一个。**

和arguments总是反应总的参数个数不同，剩余参数的长度是真实反应剩余参数的个数。

剩余参数不能用在`setter`里，否则会报语法错误，其实正常也不会有这种使用场景。

**箭头函数没有arguments变量，只能使用剩余参替代。**

### 展开操作符

上面使用到的`...`叫做展开操作符，常用于复制、合并对象和数组，函数传参等。

作为函数传参的时候，展开操作符并没有位置要求。

```javascript
const arr = [-1, -2, -3];

Math.max(...arr, 0);
// 相同的效果
Math.max(0, ...arr);
```

### Promise

callback和Promise是两种异步的API风格，ES6开始正式支持了Promise，由于ES7的异步函数（`await`、`async`）是基于Promise的，建议需要异步处理的地方使用[Promise](https://github.com/nzakas/understandinges6/blob/master/manuscript/11-Promises.md)。

```javascript
function sleep(time) {
    return new Promise(resolve => setTimeout(resolve, time));
}

async function doSomthing() {
    await sleep(1000);
    // do something
}
```

### 迭代器

迭代器可以使用`for...of`循环，相比数组进行迭代，迭代器有占用内存小的优势，因为迭代器的元素只有在需要时才会计算生成，不用像数组那样需要预先申请好内存。

>更详细的迭代器用法参见[Iterators-And-Generators](https://github.com/nzakas/understandinges6/blob/master/manuscript/08-Iterators-And-Generators.md)。

# Rules

1. [JavaScript Standard Style](https://standardjs.com/rules.html)
2. [我的ESLint规则](https://github.com/JiangJie/eslint-jarvis)