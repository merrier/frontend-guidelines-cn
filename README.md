# frontend-guidelines-cn

一些HTML，CSS和JS的最佳实践。翻译自bendc的[front-guidelines](https://github.com/bendc/frontend-guidelines)，同时添加了目录和自己的认识。

## 目录

<details>
<summary><b>点击展开</b></summary>
* [HTML](#html)
    * [语义化](#语义化semantics)
    * [简洁](#简洁brevity)
    * [可读性](#可读性accessibility)
    * [语言](#语言language)
    * [性能](#性能performance)
* [CSS](#css)
    * [分号](#分号semicolons)
    * [盒模型](#盒模型box-model)
    * [流布局](#流布局flow)
    * [position属性](#position属性positioning)
    * [选择器](#选择器selectors)
    * [易覆盖](#易覆盖specificity)
    * [避免覆盖](#避免覆盖overriding)
    * [继承](#继承inheritance)
    * [合并样式](#合并样式brevity)
    * [语言](#语言language-1)
    * [浏览器前缀](#浏览器前缀vendor-prefixes)
    * [动画](#动画animations)
    * [单位](#单位units)
    * [颜色](#颜色colors)
    * [避免HTTP请求](#避免http请求drawing)
    * [慎用hack](#慎用hackhacks)
* [JavaScript](#javascript)
    * [性能](#性能performance-1)
    * [无状态](#无状态statelessness)
    * [原生](#原生natives)
    * [类型转换](#类型转换coercion)
    * [循环](#循环loops)
    * [慎用arguments](#慎用argumentsarguments)
    * [慎用apply](#慎用applyapply)
    * [慎用bind](#慎用bindbind)
    * [避免嵌套](#避免嵌套higher-order-functions)
    * [避免多层嵌套](#避免多层嵌套composition)
    * [缓存](#缓存caching)
    * [变量](#变量variables)
    * [IIFE](#iifeconditions)
    * [对象遍历](#对象遍历object-iteration)
    * [Map](#mapobjects-as-maps)
    * [科里化](#科里化curry)
    * [可读性](#可读性readability)
    * [代码复用](#代码复用code-reuse)
    * [最小化依赖](#最小化依赖dependencies)
</details>

## HTML

### 语义化（Semantics）

HTML5为我们提供了大量的语义元素，旨在精确地描述内容。请充分利用这些新的元素。

> 注：在使用时需要注意兼容性以及这些元素的默认样式

```html
<!-- bad -->
<div id=main>
  <div class=article>
    <div class=header>
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- good -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime=2015-02-21>21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

确保你理解你使用的H5元素的语义。如果错误的使用，反而还不如使用传统元素。

```html
<!-- bad -->
<h1>
  <figure>
    <img alt=Company src=logo.png>
  </figure>
</h1>

<!-- good -->
<h1>
  <img alt=Company src=logo.png>
</h1>
```

### 简洁（Brevity）

保持代码简洁。忘掉你之前写XHTML时养成的习惯。

```html
<!-- bad -->
<!doctype html>
<html lang=en>
  <head>
    <meta http-equiv=Content-Type content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel=stylesheet href=style.css type=text/css />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type=email placeholder=you@email.com required=required />
    </label>
    <script src=main.js type=text/javascript></script>
  </body>
</html>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Contact</title>
  <link rel=stylesheet href=style.css>

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type=email placeholder=you@email.com required>
  </label>
  <script src=main.js></script>
</html>
```

### 可读性（Accessibility）

可访问性应该提前考虑到。你不必为了改进你的网站而成为WCAG专家，你现在可以做一些小的优化，但是它们会带来显著的变化，比如：

* 正确使用 `alt` 属性
* 确保用正确的元素来标记链接和按钮（千万不要用`<div class=button>`来标记按钮或链接）
* 不完全依赖颜色来传达信息
* 显式标注表单控件（即使用`label`）

```html
<!-- bad -->
<h1><img alt=Logo src=logo.png></h1>

<!-- good -->
<h1><img alt=Company src=logo.png></h1>
```

### 语言（Language）

虽然语言和字符编码的设置是可选的，但建议始终在`HTML`中声明，即使它们已经在HTTP报头中指定了。同时，`UTF-8`比任何其他字符编码都要好。

```html
<!-- bad -->
<!doctype html>
<title>Hello, world.</title>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Hello, world.</title>
</html>
```

### 性能（Performance）

如果没有必须在您的内容之前加载脚本的理由，千万不要阻塞页面的呈现。
如果样式比较多（即样式文件比较大），就将那些需要最先使用的样式孤立出来并优先加载，而其他样式可以放在单独的文件中并延迟加载。
两个HTTP请求会明显慢于一个，所以尽量减少HTTP请求。

```html
<!-- bad -->
<!doctype html>
<meta charset=utf-8>
<script src=analytics.js></script>
<title>Hello, world.</title>
<p>...</p>

<!-- good -->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src=analytics.js></script>
```

## CSS

### 分号（Semicolons）

语法上讲，分号是CSS中的分隔符，所以总是把它当作终止符来使用（不要省略分号）。

```css
/* bad */
div {
  color: red
}

/* good */
div {
  color: red;
}
```

### 盒模型（Box model）

盒子模型对于整个document应该是一致的。
`* { box-sizing: border-box; }`无伤大雅，如果可以避免，最好不要更改个别元素的盒模型表现。

```css
/* bad */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* good */
div {
  padding: 10px;
}
```

### 流布局（Flow）

如果可以避免，不要更改元素的默认行为。尽可能让元素保持在自然文档流中。例如，如果你只是想删除图片下面的空白，是不需要改变它的`diaplay`属性的：

```css
/* bad */
img {
  display: block;
}

/* good */
img {
  vertical-align: middle;
}
```

同样的，避免让某个元素脱离正常文档流。

```css
/* bad */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* good */
div {
  width: 100px;
  margin-left: auto;
}
```

### position属性（Positioning）

在CSS中改变元素的定位有很多方法。推荐使用像`Flexbox`和`Grid`这样的现代布局规范，这样可以避免将元素从正常文档流中移除，例如`position: absolute`。

### 选择器（Selectors）

简化选择器。当选择器中超过3个结构伪类、子类或兄弟组合时，就应该考虑向要匹配的元素添加类名来简化了。

```css
/* bad */
div:first-of-type :last-child > p ~ *

/* good */
div:first-of-type .info
```

避免不必要的选择器。

```css
/* bad */
img[src$=svg], ul > li:first-child {
  opacity: 0;
}

/* good */
[src$=svg], ul > :first-child {
  opacity: 0;
}
```

### 易覆盖（Specificity）

不要让选择器难以被覆盖，所以尽量减少`id`和`!important`的使用。

```css
/* bad */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* good */
.foo.bar {
  color: green;
}
.foo {
  color: red;
}
```

### 避免覆盖（Overriding）

覆盖样式会使样式的调试变困难，同时可读性变差，应该尽量避免。

```css
/* bad */
li {
  visibility: hidden;
}
li:first-child {
  visibility: visible;
}

/* good */
li + li {
  visibility: hidden;
}
```

### 继承（Inheritance）

有些样式会自动继承，要合理利用这一特性来简化选择器。

```css
/* bad */
div h1, div p {
  text-shadow: 0 1px 0 #fff;
}

/* good */
div {
  text-shadow: 0 1px 0 #fff;
}
```

### 合并样式（Brevity）

保持代码简洁，尝试合并多个属性。

```css
/* bad */
div {
  transition: all 1s;
  top: 50%;
  margin-top: -10px;
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 20px;
  padding-left: 10px;
}

/* good */
div {
  transition: 1s;
  top: calc(50% - 10px);
  padding: 5px 10px 20px;
}
```

### 语言（Language）

在表示旋转角度时，推荐使用英文的“圈数”（turn），会比角度（deg）更具语义化

```css
/* bad */
:nth-child(2n + 1) {
  transform: rotate(360deg);
}

/* good */
:nth-child(odd) {
  transform: rotate(1turn);
}
```

### 浏览器前缀（Vendor prefixes）

有些浏览器前缀已经过时了，不要继续使用了，而如果真的需要添加前缀，请插在标准属性之前。

```css
/* bad */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* good */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

### 动画（Animations）

有利于动画的过渡。避免动画比“不透明”和“变换”其他属性。

相比动画，CSS3中的`trasition`更推荐使用，但是避免对除`opacity`和`transform`之外的属性进行动画。

```css
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### 单位（Units）

尽可能使用纯数字来表示单位。如果使用相对的单位，推荐使用`rem`。秒要比毫秒更好。

```css
/* bad */
div {
  margin: 0px;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}

/* good */
div {
  margin: 0;
  font-size: .9rem;
  line-height: 1.5;
  transition: .5s;
}
```

### 颜色（Colors）

如果需要透明度，推荐使用`rgba`。否则，总是使用十六进制格式来表示颜色。

```css
/* bad */
div {
  color: hsl(103, 54%, 43%);
}

/* good */
div {
  color: #5a3;
}
```

### 避免HTTP请求（Drawing）

如果可以用CSS实现某个效果时，避免使用图片，这样可以避免不必要的HTTP请求。

```css
/* bad */
div::before {
  content: url(white-circle.svg);
}

/* good */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

### 慎用hack（Hacks）

慎用Hack。

```css
/* bad */
div {
  // position: relative;
  transform: translateZ(0);
}

/* good */
div {
  /* position: relative; */
  will-change: transform;
}
```

## JavaScript

### 性能（Performance）

对可读性、正确性和表现性的要求会高于性能。JavaScript这门语言本身永远不会成为你的性能瓶颈。一些像图片压缩、减少HTTP请求和避免DOM重绘之类的优化方式会对性能产生很大的影响。

```javascript
// bad (albeit way faster)
const arr = [1, 2, 3, 4];
const len = arr.length;
var i = -1;
var result = [];
while (++i < len) {
  var n = arr[i];
  if (n % 2 > 0) continue;
  result.push(n * n);
}

// good
const arr = [1, 2, 3, 4];
const isEven = n => n % 2 == 0;
const square = n => n * n;

const result = arr.filter(isEven).map(square);
```

### 无状态（Statelessness）

尽量保持你的函数纯净。理想情况下，所有的函数都应该不产生副作用，不使用外部数据，并返回新的对象，而不改变现有的对象。

```javascript
// bad
const merge = (target, ...sources) => Object.assign(target, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }

// good
const merge = (...sources) => Object.assign({}, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }
```

### 原生（Natives）

尽可能地用原生提供的方法

```javascript
// bad
const toArray = obj => [].slice.call(obj);

// good
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

### 类型转换（Coercion）

有时候隐式类型转换会带来方便，所以不用刻意追求`===`。

```javascript
// bad
if (x === undefined || x === null) { ... }

// good
if (x == undefined) { ... }
```

### 循环（Loops）

不要使用循环，因为它们会强迫你使用可变对象。推荐使用`array.prototype`原型方法。

```javascript
// bad
const sum = arr => {
  var sum = 0;
  var i = -1;
  for (;arr[++i];) {
    sum += arr[i];
  }
  return sum;
};

sum([1, 2, 3]); // => 6

// good
const sum = arr =>
  arr.reduce((x, y) => x + y);

sum([1, 2, 3]); // => 6
```

如果你真的没办法避免循环，或者没办法使用`array.prototype`原型方法，那么推荐使用递归。

```javascript
// bad
const createDivs = howMany => {
  while (howMany--) {
    document.body.insertAdjacentHTML("beforeend", "<div></div>");
  }
};
createDivs(5);

// bad
const createDivs = howMany =>
  [...Array(howMany)].forEach(() =>
    document.body.insertAdjacentHTML("beforeend", "<div></div>")
  );
createDivs(5);

// good
const createDivs = howMany => {
  if (!howMany) return;
  document.body.insertAdjacentHTML("beforeend", "<div></div>");
  return createDivs(howMany - 1);
};
createDivs(5);
```

这里有一个[泛型循环函数示例](https://gist.github.com/bendc/6cb2db4a44ec30208e86)，它可以让递归变得更容易使用。

### 慎用argument（Arguments）

忘掉`arguments`对象。REST参数总是更好的选择，因为：

1。它不是匿名的，所以它可以让你更好地了解函数所期望的参数。
2。它是一个真正的数组，这使得它更容易使用。

```javascript
// bad
const sortNumbers = () =>
  Array.prototype.slice.call(arguments).sort();

// good
const sortNumbers = (...numbers) => numbers.sort();
```

### 慎用apply（Apply）

避免使用`apply()`，推荐使用扩展运算符。

```javascript
const greet = (first, last) => `Hi ${first} ${last}`;
const person = ["John", "Doe"];

// bad
greet.apply(null, person);

// good
greet(...person);
```

### 慎用bind（Bind）

避免使用`bind()`。

```javascript
// bad
["foo", "bar"].forEach(func.bind(this));

// good
["foo", "bar"].forEach(func, this);
```
```javascript
// bad
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = function() {
      return `${this.first} ${this.last}`;
    }.bind(this);
    return `Hello ${full()}`;
  }
}

// good
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = () => `${this.first} ${this.last}`;
    return `Hello ${full()}`;
  }
}
```

### 避免嵌套（Higher-order functions）

避免函数嵌套。

```javascript
// bad
[1, 2, 3].map(num => String(num));

// good
[1, 2, 3].map(String);
```

### 避免多层嵌套（Composition）

避免多个函数嵌套调用。

Avoid multiple nested function calls. Use composition instead.

```javascript
const plus1 = a => a + 1;
const mult2 = a => a * 2;

// bad
mult2(plus1(5)); // => 12

// good
const pipeline = (...funcs) => val => funcs.reduce((a, b) => b(a), val);
const addThenMult = pipeline(plus1, mult2);
addThenMult(5); // => 12
```

### 缓存（Caching）

对比较大的数据结构和影响性能的操作进行缓存。

```javascript
// bad
const contains = (arr, value) =>
  Array.prototype.includes
    ? arr.includes(value)
    : arr.some(el => el === value);
contains(["foo", "bar"], "baz"); // => false

// good
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(["foo", "bar"], "baz"); // => false
```

### 变量（Variables）

在变量声明的使用上，`const` > `let` > `var`。

```javascript
// bad
var me = new Map();
me.set("name", "Ben").set("country", "Belgium");

// good
const me = new Map();
me.set("name", "Ben").set("country", "Belgium");
```

### IIFE（Conditions）

推荐使用立即执行函数（IIFE），`return`语句要优于`if`、`else if`、`else`和`switch`语句。

```javascript
// bad
var grade;
if (result < 50)
  grade = "bad";
else if (result < 90)
  grade = "good";
else
  grade = "excellent";

// good
const grade = (() => {
  if (result < 50)
    return "bad";
  if (result < 90)
    return "good";
  return "excellent";
})();
```

### 对象遍历（Object iteration）

避免使用`for...in`。

```javascript
const shared = { foo: "foo" };
const obj = Object.create(shared, {
  bar: {
    value: "bar",
    enumerable: true
  }
});

// bad
for (var prop in obj) {
  if (obj.hasOwnProperty(prop))
    console.log(prop);
}

// good
Object.keys(obj).forEach(prop => console.log(prop));
```

### Map（Objects as Maps）

虽然大家已经非常习惯使用`Object`，但是`Map`通常是更好的选择。当你不知道哪个更好时，使用`Map`吧。

```javascript
// bad
const me = {
  name: "Ben",
  age: 30
};
var meSize = Object.keys(me).length;
meSize; // => 2
me.country = "Belgium";
meSize++;
meSize; // => 3

// good
const me = new Map();
me.set("name", "Ben");
me.set("age", 30);
me.size; // => 2
me.set("country", "Belgium");
me.size; // => 3
```

### 科里化（Curry）

对于许多开发者来说，科里化是一个很强大但是比较陌生的概念，所以不要滥用它。

```javascript
// bad
const sum = a => b => a + b;
sum(5)(3); // => 8

// good
const sum = (a, b) => a + b;
sum(5, 3); // => 8
```

### 可读性（Readability）

不要用看似聪明的技巧来降低代码的可读性。

```javascript
// bad
foo || doSomething();

// good
if (!foo) doSomething();
```
```javascript
// bad
void function() { /* IIFE */ }();

// good
(function() { /* IIFE */ }());
```
```javascript
// bad
const n = ~~3.14;

// good
const n = Math.floor(3.14);
```

### 代码复用（Code reuse）

不要害怕创建很多小的、高度可组合的和可重用的函数。

```javascript
// bad
arr[arr.length - 1];

// good
const first = arr => arr[0];
const last = arr => first(arr.slice(-1));
last(arr);
```
```javascript
// bad
const product = (a, b) => a * b;
const triple = n => n * 3;

// good
const product = (a, b) => a * b;
const triple = product.bind(null, 3);
```

### 最小化依赖（Dependencies）

最小化依赖。你很难知道第三方代码的具体实现逻辑。所以如果你只需要某个库中的有限方法，不要引入整个库。

```javascript
// bad
var _ = require("underscore");
_.compact(["foo", 0]));
_.unique(["foo", "foo"]);
_.union(["foo"], ["bar"], ["foo"]);

// good
const compact = arr => arr.filter(el => el);
const unique = arr => [...new Set(arr)];
const union = (...arr) => unique([].concat(...arr));

compact(["foo", 0]);
unique(["foo", "foo"]);
union(["foo"], ["bar"], ["foo"]);
```