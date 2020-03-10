1.以下代码执行后，分别弹出来的值是：

```
var num1=1; 

function testf1(){ 

​    num1=2; var num1; alert(num1); 

} 

function testf2(){ 

​    alert(num1); 

} 

testf1(); 

testf2();
```

A 2,1
B 2,2
C undefined,undefined
D 2，undefined
正确答案: A
解析:
testf1函数先执行, 先进行变量提升, 且里面的num1为私有变量,不会更改全局变量
testf2 里面的num1为全局变量

２.以下代码运行后弹出的结果是 var a = 888; ++a; alert(a++);
A 888
B 889
C 890
D 891
正确答案: B
解析:
889
a++和++a 都属于自增运算符，区别是对变量a的值进行自增的时机不同。a++是先进行取值，后进行自增。++a是先进行自增，后进行取值。
３.分析下段代码输出结果是 

```
var t = 10; 

function test(test){

​     t = t + test;

​      var t = 3;

​       console.log(t); 

​    } 

​    test(t);
```

A 6
B 3
C 13
D NaN
正确答案: B



４.代码的输出结果是 function add(){ var sum = 1 + 2; console.log( sum ); } console.log(sum) ;
A 输出两个3
B 输出一个3
C 程序报错
D 输出一个undefined
正确答案: C
解析:
sum是局部变量，全局环境未定义

５.以下代码运行的结果是 var num =5; function fn(){ num =1; } fn(); alert( num );
A 1
B undefined
C 报错
D 5
正确答案: A
解析:
函数中的num是全局的；函数执行把全局num改为了1；所以打印是1

６.以下代码运行的结果是输出 var a = b = 10; (function(){ var a=b=20})(); console.log(b);
A 10
B 20
C 报错
D undefined
正确答案: B
解析:
函数中的b是全局的；给全局b重新赋值20,所以结果是20

７.以下哪个单词不属于javascript关键字
A break
B float
C var
D return
正确答案: B
解析:
float是JavaScript的保留字，不是关键字

８.有下列代码： var x = 10; function f1( num ){ ++ num; } var y = f1( x ); console.log( ‘x=’ + x + ‘; y=’ + y ); 执行后的结果是
A x=10; y=undefined
B x=10; y=11
C x=11; y=11
D x=11; y=undefined
正确答案: A
解析:
x=10; y=undefined
９.如下代码，会弹出来是什么 var num1=“hi” var num2=1 alert(num1-num2);
A hi
B 1
C NaN
D 报错
正确答案: C
解析:
进行Number()隐士转换, 都转换为数字, num1不是数字字符串,会转换为NaN



10.HTML页面中写JavaScript时，会用到HTML的一个标签，它是
A script
B style
C link
D body
正确答案: A

11.下面选项中数据类型为数字类型的有
A “1”
B 1
C true
D NaN
正确答案: B,D

12.下面声明变量及赋值的写法正确的有
A var _myName = 12a
B var _myName = “12a”
C var myName = 12
D var $myName = true
正确答案: B,C,D
解析:
A选项 12a是字符串，不是纯数字，字符串要用引号

13.已知字符串"hello everybody "通过下面哪个方法可以去掉字符串两端的空白
A trim()
B split()
C reverse()
D join()
正确答案: A
解析:
使用排除法：split分割字符串；reverse反转数组；join将数组转成字符串

trim() 去除字符串两端空格
14.3秒后，图片消失，应该使用下面哪个方法
A hide()
B setTimeout()
C ClearInterval
D clearTimeout()
正确答案: B
解析:
setTimeout延时器

14.var arr = [5,3,2,4]; var res = arr.splice(0); console.log(res) 结果是
A [5,3,2,4]
B [5]
C [5,3]
D [2,4]
正确答案: A

[`splice(index, count_to_remove, addElement1, addElement2, ...)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)从数组移出一些元素，（可选）并替换它们。

```js
var myArray = new Array ("1", "2", "3", "4", "5");
myArray.splice(1, 3, "a", "b", "c", "d"); 
// myArray is now ["1", "a", "b", "c", "d", "5"]

```

15.var arr = [3,6,8,9]; var res = arr.slice(1,3); console.log(res) 结果是
A [6,8,9]
B [6,8]
C [3,6,8]
D [3,6]
正确答案: B
解析:
slice() 方法可从已有的数组中返回选定的元素。
23.var arr = [1,2,3,4]; arr.splice(1,0,[1,2,3,4],5); alert(arr.length);
A 4
B 5
C 6
D 9
正确答案: C
解析:
删除0个插入2个，所以最后length为6，插入的数组算一个

24.var arr = [1,2,3,4]; alert(arr.concat(1,2,3,[1,2,3],5).length)
A 4
B 9
C 11
D 5
正确答案: C
解析:
concat返回的新数组立马length,所以输出的是最新的数组长度

28.var arr = [1,2,3,4]; arr.join("&"); console.log(arr);
A [1,2,3,4]
B 1&2&3&4
C [1&2&3&4]
D 1,2,3,4
正确答案: A
解析:
join 不改变原数组。

29.var arr = [3,4,5]; var res = arr.unshift(1,2); console.log(res); 输出的结果是
A [1,2]
B [3,4,5]
C 5
D [1,2,3,4,5]
正确答案: C
解析:
unshift()执行后返回值为新数组的长度

30.下面打印出来的值为 var i = 0; [1,2,3].some(function(item,idx){ i = idx; return item == 2; }) console.log(i)
A 0
B 1
C 2
D 3

# 正确答案: B

解析:
找到满足条件为true，就停止查找

基础面试考试题２：多选题