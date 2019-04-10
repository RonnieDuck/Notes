#ES6编程风格
###*块级作用域*
- let取代var
- 全局常量和线程安全  
在let和const之间，建议优先使用const，尤其是在全局环境，不应该设置变量，只应设置常量。这符合函数式编程思想，有利于将来的分布式运算。const声明常量还有两个好处，一是阅读代码的人立刻会意识到不应该修改这个值，二是防止了无意间修改变量值所导致的错误。所有的函数都应该设置为常量。

###*字符串*
- 静态字符串一律使用单引号或反引号，不使用双引号。动态字符串使用反引号。
```js
// bad
const a = "foobar";
const b = 'foo' + a + 'bar';
// acceptable
const c = `foobar`;
// good
const a = 'foobar';
const b = `foo${a}bar`;
const c = 'foobar';
```

###*解构赋值*
- 使用数组成员对变量赋值时，优先使用解构赋值。
```js
const arr = [1, 2, 3, 4];
// bad
const first = arr[0];
const second = arr[1];
// good
const [first, second] = arr;
```
- 函数的参数如果是对象的成员，优先使用解构赋值。
```js
// bad
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;
}
// good
function getFullName(obj) {
  const { firstName, lastName } = obj;
}
// best
function getFullName({ firstName, lastName }) {
}
```
- 如果函数返回多个值，优先使用对象的解构赋值，而不是数组的解构赋值。这样便于以后添加返回值，以及更改返回值的顺序。
```js
// bad
function processInput(input) {
  return [left, right, top, bottom];
}
// good
function processInput(input) {
  return { left, right, top, bottom };
}
const { left, right } = processInput(input);
```

###*对象*
- 单行定义的对象，最后一个成员不以逗号结尾。多行定义的对象，最后一个成员以逗号结尾。
- 对象尽量静态化，一旦定义，就不得随意添加新的属性。如果添加属性不可避免，要使用Object.assign方法。
```js
// bad
const a = {};
a.x = 3;
// if reshape unavoidable
const a = {};
Object.assign(a, { x: 3 });
// good
const a = { x: null };
a.x = 3;
```

###*数组*
- 使用扩展运算符（...）拷贝数组。
```js
// bad
const len = items.length;
const itemsCopy = [];
let i;
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}
// good
const itemsCopy = [...items];
```
- 使用Array.from方法，将类似数组的对象转为数组。
```js
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

###*函数*
- 立即执行函数可以写成箭头函数的形式。
```js
(() =>- {
  console.log('Welcome to the Internet.');
})();
```
- 那些需要使用函数表达式的场合，尽量用箭头函数代替。因为这样更简洁，而且绑定了this。
```js
// bad
[1, 2, 3].map(function (x) {
  return x * x;
});
// good
[1, 2, 3].map((x) =>- {
  return x * x;
});
// best
[1, 2, 3].map(x =>- x * x);
```
- 使用默认值语法设置函数参数的默认值。
```js
// bad
function handleThings(opts) {
  opts = opts || {};
}
// good
function handleThings(opts = {}) {
  // ...
}
```

###*模块*
- 如果模块只有一个输出值，就使用export default，如果模块有多个输出值，就不使用export default，不要export default与普通的export同时使用。
- 如果模块默认输出一个函数，函数名的首字母应该小写。
- 如果模块默认输出一个对象，对象名的首字母应该大写。

- *Reference:*
>- [ECMAScript 6简介](http://es6.ruanyifeng.com/#docs/intro)

#循环中的异步代码
- 问题例子
```js
for (let i of obj){
  supertest.get('url').end(function(err,res){
    //这里不能获取对应的i
  });
}
```
- 建议解决方法 1: 嵌套闭包，性能差
```js
for (let i of obj){
  (function(i){
    supertest.get('url').end(function(err,res){
      //这里可以获取对应的i
    });
  })(i);
}
```
- 建议解决方法 2: 使用Promise.all()
```js
for (let i of obj){
  let array = [];
  //将Promise对象加入数组当中
  array.push(new Promise(function(resolve,reject){
    //需要用到的变量在该处赋值
    let params = i;
    //本次循环需要执行的操作
  }));
}
await Promise.all(function(result){
  //按顺序排列的Promise的返回结果的数组
});
```

#JavaScript的原型链、继承（干货）
- *Reference:*
>- [Javascript继承机制的设计思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)
>- [Javascript 面向对象编程（一）：封装](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html)
>- [Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)
>- [Javascript面向对象编程（三）：非构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)