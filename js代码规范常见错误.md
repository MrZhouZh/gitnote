# js代码风格

## 一. 变量相关

(1)变量数量的定义

NO: 滥用变量:

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

NO: 自我