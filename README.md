# 命名

### 变量、函数命名

命名尽量使用浅显易懂的英文单词，较长单词可使用缩写，
对于不容易理解的变量名多写注释。

普通变量使用驼峰形式，
常量全大写，单词之间用`_`隔开。

```javascript
const GAME_NAME = 'douniu';
const playerName = 'liming';
function playGame() {
    console.log(`${playerName} is playing ${gameName}`);
}
```

### 变量声明

不可修改变量必须使用`const`声明，
不会修改变量尽量使用`const`声明，
可修改变量使用`let`声明，
一个变量一个const或者let声明，
禁止使用~~var~~，因为存在变量提升、变量重复声明、变量覆盖等隐藏的棘手问题。

```javascript
const a = 1;
let b = 1;
let c;
for(let i = 0, i < n; i++) {

}
```

# 格式相关

### 缩进

使用`空格`缩进，一个tab设置为2个或者4个空格，需和团队保持一致。
杜绝混用空格和tab。

### 单行长度

传统建议一行不超过80字符，具体可视显示器而定。

### 分号

表达式行尾必须加分号，
`if`、`while`等循环、class、function声明等行尾可不加，
函数以赋值方式声明的需要加，以单纯声明方式的不用加。

```javascript
const fn = function() {};
function fn1() {}
```

### 花括号

左花括号跟在行尾，右花括号另起一行，参考前面的代码。

if else 里面如果只有一行语句，不用再写花括号了，也可以使用三元表达式。

以上非功能性语法格式可以借助IDE或者`jsFormat`等插件进行格式化。

# 语法相关

### 字符串

简单字符串变量使用单引号，内容含有单引号的字符串使用双引号，单双引号都有的字符串使用反引号```。
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

通常情况直接使用字面量，最后一个元素后面不能有逗号。
对象的key如果是一个合法的变量名就不加引号，否则加单引号

```javascript
const arr = [1, 2, 3];
const obj = {
    a: 1,
    // bad
    'b': 2,
    // good
    'c-d': 3 // 这里不要留逗号，在一些低级的解释器里会出错
}
```

### null和undefined

null和undefined都是一种空值（通过布尔类型转换!!为false的值），两者容易混用，不过其实两者有各自的使用场景。
null表示没有值，通常用在不应有值的地方，意思就是不用怀疑，此处就是不应该有值，如果有值那就是出错了。
举个栗子，比如进行一次网络请求，如果结果成功了，传给回调函数的第一个参数应该为null，回调函数认为该参数是一个error，如果没有error，那就是成功了。

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

而undefined表示一个未定义的值，意思是此处其实有值，但是未定义，通常不需要关心其具体内容。
比如用在函数返回值、变量声明未定义、访问不存在的对象属性等。

# ES6

### let和const

### export和import

ES6终于在官方层面支持了JS的模块化语法，不同于AMD、CMD、UMD、commonjs等野鸡模块化语法，推荐使用官方版。

### 箭头函数

箭头函数解决了函数在运行时存在this不确定的问题，让函数在声明的时候就确定了this，而且不会在运行时改变。
多使用箭头函数总是好的，特别是一些使用短函数的场景。

```javascript
const arr = [1, 2, 3].map(number => number * number);
```

### class

需要类的地方使用class进行类声明，不推荐继续使用原型链。

类成员函数只能使用简写语法，同理，建议对象的函数属性也是用简写语法。

```javascript
class DN {
    play() {
        console.log('enjoy!');
    }
}

const DN = {
    play() {
        console.log('enjoy!');
    }
};
```
