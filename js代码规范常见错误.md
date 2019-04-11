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
const cityZipCodeRegx = /^[^,\]+[,\s]+(.+?)s*(d{5})$/
```
