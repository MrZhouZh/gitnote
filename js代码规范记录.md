# js代码风格

## 一. 变量相关

(1)变量数量的定义

No: 滥用变量:

```js
let kpi = 4; // 定义好后一直未用过
function example() {
  var a = 1;
  var b = 2;
  var c = a + b;
  var d = c + 1;
  var e = d + a;
  return e;
}
```

Yes: 数据只是用一次或不适用就无需装到变量中

```js
let kp = 4; // 没用的就删除掉, 不然过三个月子级都不敢删
function example() {
  var a = 1;
  var b = 2;
  return 2a + b + 1;
}
```

(2)变量的命名

No: 自我感觉良好的缩写

```js
let fName = 'jack';  // 看起来命名规范, 缩写,驼峰命名, eslint各种检测工具都能通过, 但是fName 是个什么? 这时候你就想说, What are you 弄啥嘞
```

Yes: 无需对每个变量都写注释, **见名识义**

```js
let firstName = 'jack';
```

No: 命名过于啰嗦

```js
let nameString;
let theUsers;
```

Yes: 做到简洁明了

```js
let name;
let users;
```

(3)特定的变量

No: 无说明的参数

```js
if(value.length < 8) { // 为什么要小于8, 8表示啥??长度还是位移,还是高度?!!!
  ...
}
```

Yes: 添加变量

```js
const MAX_INPUT_LENGTH = 8;
if(value.length < MAX_INPUT_LENGTH) {	// 限定最大输入长度
  ...
}
```

(4)使用说明性的变量(即有意义的变量名)

No: 长代码不知道啥意思

```js
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegx = /^[^,\]+[,\s]+(.+?)s*(d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegx)[1], // 这个公式要做什么用??原作者已离职.自己看代码
  address.match(cityZipCodeRegx)[2]
)
```

Yes: 用变量名来解释长代码的含义

```js
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegx = /^[^,\]+[,\s]+(.+?)s*(d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegx) || [];
saveCityZipCode(city, zipCode);
```

(5) 避免使用太多的全局变量

No: 在不同的文件不停的定义全局变量

```js
// name.js
window.name = 'a';

// hello.js
window.name = 'b';

// time.js
window.name= 'c'; // 三个文件的加载顺序会导致输出的结果不同.同时, 你对window.name的修改了都有可能不生效, 因为你不知道你修改过之后别人是不是又在别的说明文件中对其的值重置了.所以随着文件的增多,会导致一团乱麻.
```

Yes: 少用或使用替代方案,你可以选择只用局部变量.通过(){}方法

> 如果你确实用很多ed全局变量需要共享, 你可以使用vuex,redux或者你自己参考flux模式写一个也行.

(6)变量赋值

No: 对于求值获取的变量, 没有兜底.

```js
const MIN_NAME_LENGTH = 8;
let lastName = fullName[1];
if(lastName.length > MIN_NAME_LENGTH) {	// 这样你就成功的给自己挖了一个坑,你有考虑如果fullName = ['jack']这样的情况吗?这种程序一跑起来就要炸~
  ...
}
```

Yes: 对于求值变量, 做好兜底

```js
const MIN_NAME_LENGTH = 8;
let lastName = fullName[1] || '';  // 做好兜底, fullName[1]取不到时, 不至于赋值个undefined,至少有个空字符,从根本上讲, lastName的类型还是String,String的原型链上的特性都能使用不会报错, 也不会变成undefined.
if(lastName.length > MIN_NAME_LENGTH) {
  ...
}
// 其实在项目中有很多的求值变量, 对于每个求值变量都需要做好兜底.
let prototypeValue = Object.attr || 0;	// 因为Object.attr有可能为空, 所以需要兜底.但是变量赋值就不需要兜底了.
let a = 2;
let myName = 'Tiny';
```

## 二. 函数相关

（1）函数命名

No: 从命名无法知道返回值类型

```js
function showFriendsList(){ // 现在问，你知道这个返回的是一个数组，还是一个对象，还是true or false。你能答的上来否？你能答得上来我请你吃松鹤楼的满汉全席还请你不要当真。
  ...
}
```

Yes: 对于返回true or false的函数，最好以should/is/can/has开头

```js
functionshouldShowFriendsList() {
  ...
}
function isEmpty() {
  ...
}
function canCreateDocuments() {
  ...
}
function hasLicense() {
  ...
}
```

(2) 功能函数最好为纯函数

No: 不要让功能函数的输出变化无常。

```js
function plusAbc(a, b, c) { // 这个函数的输出将变化无常，因为api返回的值一旦改变，同样输入函数的a，b,c的值，但函数返回的结果却不一定相同。
  var c = fetch('../api');
  return a + b + c;
}
```

Yes: 功能函数使用纯函数，输入一致，输出结果永远唯一

```js
function plusAbc(a, b, c) {	// 同样输入函数的a,b,c的值, 但函数返回的结果永远相同
  return a + b + c;
}
```

(3) 函数传参

No: 传参无说明

```js
page.getSVG(api, true, false); // true和false啥意思，一目不了然
```

Yes: 传参有说明

```js
page.getSVG({
  imageApi: api,
  includePageBackground: true,  // 一目了然，知道这些true和false是啥意思
  compress: false
})
```

(4)动作函数要以动词开头

No: 无法识别函数的意图

```js
function emlu(user) {
 ...
}
```

Yes: 动词开头, 意图明显

```js
function sendEmailToUser(user) {
  ...
}
```

(5) 一个函数完成一个独立的功能，不要一个函数混杂多个功能

> 这是软件工程中最重要的一条规则，当函数需要做更多的事情时，它们将会更难进行编写、测试、理解和组合。当你能将一个函数抽离出只完成一个动作，他们将能够很容易的进行重构并且你的代码将会更容易阅读。如果你严格遵守本条规则，你将会领先于许多开发者。

No: 函数功能混乱, 一个函数包含多个功能.最后就像能以一当百的老师傅(老程序猿)一样, 被乱拳(功能复杂函数)打死

```js
function sendEmailToClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if(clientRecord.isActive()) {
      email(client)
    }
  })
}
```

Yes: 功能拆解

```js
function sendEmailToActiveClients(clients) {	// 拆解利于维护和复用
  clients.filter(isActiveClient).forEach(email);
}
function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

(6) 优先使用函数式编程

No: 使用`for`循环编程

```js
for(var i = 0; i <= 10; i++) {	// 一看到for循环让人顿生不想看的情愫
  a[i] = a[i] + 1;
}
```

Yes: 使用函数式编程

```js
let b = a.map(item => ++item)	// 现在javascript中几乎所有的for循环都可以被 map, filter, find, some, any, forEach等函数式编程取代
```

(7) 函数中过多使用if...else...

No: if...else...过多

```js
if(a === 1) {
  ...
} else if(a === 2) {
  ...
} else if(a === 3) {
  ...
} else {
  ...
}
```

Yes: 可以使用 `switch` 替代或用数组对象替代

```js
switch(a) {
  case 1:
    ...
  case 2:
    ...
  case 3:
    ...
  default:
    ...
}
// or
let handler = {
  1: () => { ... }
  2: () => { ... }
  3: () => { ... }
  default: () => { ... }
}

handler[a]() || handler['default']()
```

## 三.ES6/ES7语法

(1) 尽量使用箭头函数

No: 采用传统函数

```js
function foo() { ... }
```

Yes: 使用箭头函数

```js
let foo = () => { ... }
```

(2) 连接字符串

No: 采用传统 `+` 号

```js
var message = 'Hello ' + name + ', it\'s ' + time + ' now.'
```

Yes: 使用模板字符

```js
let message = `Hello ${name}, it's ${time} now.`
```

(3) 使用解构赋值

No: 采用传统赋值

```js
var data = { name: 'dys', age: 1 };
var name = data.name;
var age = data.age;

var fullName = ['jack', 'willen'];
var firstName = fullName[0];
var lastName = fullName[1];
```

Yes: 使用解构赋值

```js
let data = { name: 'dys', age: 1 };
let {name, age} = data;

let fullName = ['jack', 'willen'];
let [firstName, lastName] = fullName;
```

(4) 尽量使用类`class`

No: 采用传统的函数原型链实现继承

```js
典型的 ES5 的类(function)在继承,构造和方法定义方面可读性较差, 当需要继承时, 代码太多, 优先使用 class ,就省略了大部分代码.
```

Yes: 采用`ES6`类实现继承

```js
class Animal {
  constructor(age) {
    this.age = age
  }
  
  move() {
    ...
  }
}

class Mammal extend Animal {
  constructor(age, furColor) {
    super(age)
    this.furColor = furColor
  }
  
  liveBirth() {
    ...
  }
}

class Human extend Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor)
    this.languageSpoken = languageSpoken
  }

  speak() {
    ...
  }
}
```

***To be continued...***
