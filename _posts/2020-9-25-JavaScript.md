## 嵌入html

javascript 一般放在head 中

```html
<html>
<head>
  <script>
    alert('Hello, world');
  </script>
</head>
<body>
  ...
</body>
</html>
```

放入单独.js

```
<html>
<head>
  <script src="/static/js/abc.js"></script>
</head>
<body>
  ...
</body>
</html>
```

## 变量

```
var x = 1;
```

可以赋值不同类型，只能一次声明

并不强制var，但是没有var，会成全局变量

应在第一行声明 'use strict'; 

const

解构赋值 

~~~javascript
var [x, y, z] = ['hello', 'JavaScript', 'ES6']; 
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name, age, passport} = person;
~~~

嵌套对象属性

```
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip==0}} = person;//还可以使用默认赋值
```

如果已经声明过，要加括号

```
({x, y} = { name: '小明', x: 100, y: 200});
```

交换

```
var x=1, y=2;
[x, y] = [y, x]
```

## 相等

== 会自动转换类型，有时会得到诡异的结果

=== 尽量使用这个



## null undefined

null表示空 undefined表示未定义



## 数组

[ ] 或者 new Array()

用索引 arr[0] ，不会越界

IndexOf() slice()返回部分

unshift()往头部增加元素 shift()删除

sort() reverse() 

splice()万能操作法

```
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

concat 连接两个数组，不修改，返回新的，自动拆开Array

join

```
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```

自动转换成字符串

## 对象

```
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```

键是字符串 值是任意类型

获取对象用 类名.键 也可以索引 ['age']

可以随意增加 删除 (delete)属性

可以用in 检测有没有属性 也有可能是继承来的属性

```
'name' in xiaoming; 
```

hasOwnProperty()可以排除掉继承

### 方法

```
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了
```

this有时会指向全局变量，我感觉[教程](https://www.liaoxuefeng.com/wiki/1022910821149312/1023023754768768)这一段挺迷的，不懂他想说什么。方法不一直都是只有先声明class才能用 

```
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this; // 在方法内部一开始就捕获this
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth; // 用that而不是this
        }
        return getAgeFromBirth();
    }
};

xiaoming.age();
```

apply 函数 来绑定this 第二个参数是Array 表示函数本来的参数

```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

call函数与apply很像，至少他是按顺序

```
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

装饰器 动态改变函数

```
window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};
```

## 字符串

### 多行字符串

```
`这是一个
多行
字符串`;
```

用esc下的反引号

### 模板字符串

```
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```

### 操作字符串

利用索引访问

字符串无法修改，使用方法会返回新字符串

toUpperCase() toLowerCase() IndexOf substring()

substring(n) 从n开始 不包括

## 循环

for

```
for (i=1; i<=10000; i++) {
    x = x + i;
}
```

for in

```
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    console.log(key); // 'name', 'age', 'city'
}
```

要过滤掉对象继承的属性，用`hasOwnProperty()`来实现：

```
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    if (o.hasOwnProperty(key)) {
        console.log(key); // 'name', 'age', 'city'
    }
```

```
var a = ['A', 'B', 'C'];
for (var i in a) {
    console.log(i); // '0', '1', '2'
    console.log(a[i]); // 'A', 'B', 'C'
}
```

Array遍历得到的是string

while

```
var x = 0;
var n = 99;
while (n > 0) {
    x = x + n;
    n = n - 2;
}
x; // 2500
```

do while

```
var n = 0;
do {
    n = n + 1;
} while (n < 100);
n; // 100
```

for of循环

```
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    console.log(x);
}
for (var x of s) { // 遍历Set
    console.log(x);
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
}
```

for in会访问集合所有属性，而for of只会访问容器本身元素

for each循环

~~~JavaScript
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
});
~~~

set 前两个相同

map 前两个value、key



## Map Set

```
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```

set has delete

```
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```

set会过滤相同的 add delete

## 函数

```
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

```
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```

可以传入比所需的多，无影响。也可以少，会收到undefined

arguments 类似与其他语言的可变形参。他类似于Array，包含所有参数

JavaScript会在行末自动添加分号



变量提升

```
'use strict';

function foo() {
    var x = 'Hello, ' + y;
    console.log(x);
    var y = 'Bob';
}

foo();
```

var y;会提前，结果会变成 Hello,undefined

尽量在函数前定义变量



全局变量

window，变量都是它的属性

名字空间

```
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

用let可以声明 块级作用域

```
function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError:
    i += 1;
}
```

### 高阶函数

```
function add(x, y, f) {
    return f(x) + f(y);
}
```

#### map reduce

map作用于每个

~~~javascript
'use strict';

function pow(x) {
    return x * x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);
~~~

reduce [x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)

```
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25
```

#### filter

返回值为真 保留

```
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```

#### sort

包括数组，也是按字典序    会直接修改

~~~javascript
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
~~~

#### every

~~~javascript
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true, 因为每个元素都满足s.length>0
~~~

#### find

~~~javascript
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.find(function (s) {
    return s.toLowerCase() === s;
})); // 'pear', 因为pear全部是小写
~~~

#### findIndex

返回索引

### 闭包

```
function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}
var f = lazy_sum([1,2,3,4,5])//并不会计算
f();//会计算
```

