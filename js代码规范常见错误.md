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

## 二. 