# 变量

### 变量命名

变量包括普通变量、成员变量、类变量、函数。

尽量命名使用浅显易懂的英文单词组合而成，较长单词可使用缩写，对于不容易理解的变量名应多写注释。

一般变量使用驼峰形式命名，常量使用全大写形式，并在单词之间用`_`隔开。

```javascript
const GAME_NAME = 'dn';
const playerName = 'liming';
function playGame() {
    console.log(`${playerName} is playing ${gameName}`);
}
```

### 变量定义

变量定义包括变量声明和赋值两步。

不可修改变量必须使用`const`声明，不会修改变量尽量使用`const`声明。

可修改变量使用`let`声明。

变量声明和赋值尽量一步搞定，并一行定义一个变量，即使是最简单的`let i = 0;`，禁止使用链式赋值方式，存在隐蔽的全局变量问题。

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

禁止使用~~var~~，因为存在变量提升、变量重复声明、变量覆盖等隐蔽的棘手问题。

而且，let和const都是块级作用域的，可以减少作用域冲突问题。

```javascript
const a = 1;
let b = 1;
let i;
for(i = 0, i < n; i++) {

}
```

在变量需要使用时才定义。

```javascript
function enterRoom(id) {
    if(!id) return;

    // 前面的代码根本不需要opts
    const opts = createOpts();
    _enter(id, opts);
}
```

数组和对象类型的变量，在创建的时候直接使用字面量进行创建。

如果一个对象只需要当做简单的哈希类型来使用，可以使用`Object.create(null);`来进行创建纯净的对象变量。

```javascript
const arr = [1, 2, 3];
const obj = {a: 1, b: 2};
const map = Object.create(null);
```

### 全局变量

不对全局变量进行增删改，需要跨文件共享变量请使用模块化。

# 格式相关

### 缩进

严格使用`空格`进行缩进，一个tab设置为2个或者4个空格，需和团队保持一致。

4个方面月度，不过，如果项目缩进层级较多，建议使用2个。

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

if else 里面如果只有一行语句，不用再写花括号了，这种情况也可以使用三元表达式，如果只有if，可以使用`&&`或`||`进行简写，有`return`的除外。

```javascript
isTrue ? fn1() : fn2();
isTrue && fn1();
```

switch的case和default语句使用花括号。

以上非功能性语法格式大多可以借助IDE或者`jsFormat`等插件进行格式化，达到规范的效果。

# 语法相关

### 严格模式

总是使用严格模式，文件第一行必须是`'use strict;'`。

使用ES6模块化语法自动开启严格模式。

### 字符串

简单字符串变量使用单引号，内容含有单引号的字符串使用双引号，单双引号都有的字符串使用反引号```，保证字符串里面没有使用转义。

字符串拼接使用反引号。

反引号是ES6引入的新语法，是一种模板字符串，可以内嵌变量，最大的用处就是用来取代字符串拼接。

```javascript
const world = 'world';

const str1 = 'hello, world';
const str2 = "hello', world";
const str3 = `'hello', "world"`;
const str4 = `hello ${world}`;
```

### 数组、对象

最后一个元素后面不能留逗号。

对象的key如果是一个合法的变量名就不加引号，否则加单引号。

```javascript
const arr = [1, 2, 3/* 这里不要留逗号*/];
const obj = {
    a: 1,
    // bad
    'b': 2,
    // good
    'c-d': 3 // 这里不要留逗号，在一些低级的解释器里会出错
}
```

对象的属性获取使用`.`，如果属性名含有非法变量名字符或者是一个变量，才使用`[]`。

### 函数

因为函数声明也存在变量提升的问题，应尽量使用赋值的方式定义函数，同时为函数命名会更方便查问题。

不过，如果使用模块化方式的话，function也不会提升到其他模块去了，这样的影响也不算大。

```javascript
const playGame = function playGame() {

};
```

需要注意的是，只有使用变量名才能进行函数调用，使用函数名是不行的，实际上函数名经过变量提升了，不过没有赋值。

```javascript
const playGame = function _playGame() {

};
console.log(playGame.name);
// _playGame
_playGame();
// Uncaught ReferenceError: _playGame is not defined
```

避免在if、while等语句里面定义函数，不过可以进行函数赋值。

```javascript
let fn;
if(true) {
    fn = function () {};
    // or
    fn = () => {};
}
fn();
```

函数尽早return，减少缩进，条理清晰。

```javascript
function enterRoom(id) {
    if(!id) return;
    if(!isNumber(id)) return;
    if(!isValid(id)) return;
    // enterRoom
}
```

### null和undefined

null和undefined都是一种空值（通过布尔类型转换!!为false的值），两者容易混用，不过其实两者有各自的使用场景。

null表示没有值，通常用在不应有值的地方，意思就是不用怀疑，此处就是不应该有值，如果有值那就是出错了。

举个栗子，比如进行一次网络请求，如果结果成功了，传给回调函数的第一个参数应该为null，回调函数认为该参数是一个error，如果接收到null，那就代表没有error，就是成功了。

```javascript
function callback(err, data) {

}
function request(callback) {
    // if success
    return callback(null, data);
    // if fail
    callback(new Error('message'));
}
```

再如`Object.create(null);`，传null表示创建出来的对象不应该有原型了。

而undefined表示一个未定义的值，意思是此处其实有值，但是未定义，通常不需要关心其具体内容。

比如用在函数返回值、变量声明而未定义、访问不存在的对象属性等情况。

### ++/--

避免在for循环以外使用++/--，使用+=/-=代替。

### 比较操作

使用`===`、`!==`，而不是`==`、`!=`。

if判断的表达式会自动进行类型转换，无需显式做一次判断。

使用`!`和`!!`将变量转换成布尔变量。
