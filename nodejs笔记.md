## Node.js 事件循环

1、引入模块,创建对象

```js
//引入events模块

var events=require('events');

//创建eventEmitter对象

var eventEmitter = new events.EventEmitter();
```

2、绑定事件处理程序

```js
//绑定事件及事件的处理程序

eventEmitter.on('eventname',eventHandler);
```

3、通过程序触发事件

//触发事件

```js
eventEmitter.emit('eventName');
```

实例：创建mian.js文件，代码如下：

```js
//引入events模块

var events =require('events');

//创eventEmitter对象

var eventEmitter = new events.EventEmitter();

//创建事件处理程序

var connectHandler = function connected(){

​    console.log('连接成功');

​    //触发data_received事件

​    eventEmitter.emit('data_received');

}

//绑定connection事件处理程序

eventEmitter.on('connection',connectHandler);

//使用匿名函数绑定data_received事件

eventEmitter.on('data_received',function(){

​    console.log('数据接收成功');

});

//触发connnection事件

eventEmitter.emit('connection');

console.log('程序执行完毕');
```

## node.js　Stream(流)

### 输入流：

var fs= require('fs');

var readerStream= fs.createReadStream('input.txt');



```js
var fs =require('fs');

var data = '';


*//创建可读流*

var readerStream = fs.createReadStream('input.txt');


*//设置编码为 utf-8*

readerStream.setEncoding('UTF-8');

*//处理流事件 -->data, end, and error*

readerStream.on('data',function(chunk){

​    data+=chunk;

});

 readerStream.on('end',function(){

​     console.log(data);

 });

 readerStream.on('error',function(err){

​    console.log(err.stack);

 });

 console.log("程序执行完毕");
```



### 输出流：

var fs= require('fs');

var writerStream= fs.createWriteStream('out.txt');



```js
var fs = require('fs');

 var data = 'W3Cschool教程官网地址：www.w3cschool.cn';



 *//创建一个可以写入的流。写入到文件output.txt中*

 var writerStream =fs.createWriteStream('output.txt');



 *//使用utf8编写数据*

 writerStream.write(data,'utf-8');

 *//标记文件末尾*

 writerStream.end();



 *//处理流事件 -->data,end,and error*

 writerStream.on('finish',function(){

​     console.log('写入完成！');

 });

 writerStream.on('error',function(){

​     console.log(err.stack);

 });

 console.log('程序执行完毕');


```

### 管道流：

```js
var fs=require('fs');

*//创建一个可读流*

var readerStream = fs.createReadStream('input.txt');

*//创建一个可写流*

var writerStream= fs.createWriteStream('output.txt');

*//管道读写操作*

*//读取input.txt文件内容，并将内容写入到output.txt中*

readerStream.pipe(writerStream);

console.log('程序执行完毕');
```

链式流

链式是通过连接输出流到另外一个流并创建多个对个流操作链的机制。链式流一般用于管道操作。

接下来我们就是用管道和链式来压缩和解压文件。

创建 compress.js 文件, 代码如下：

```js
var fs = require('fs');

var zlib=require('zlib');



*//压缩input.txt文件为input.txt.gz*

fs.createReadStream('input.txt')

​    .pipe(zlib.createGzip())

​    .pipe(fs.createWriteStream('input.txt.gz'));



​    console.log('文件压缩完成');
```





## node.js模块系统

node.js文件可以相互调用

1、创建hello.js文件

2、创建main.js文件，引入hello.js文件，调用其中的方法

代码如下：

1、创建文件hello.js

```js
exports.world=function(){

​    console.log('hello world');

}
```

2、在main.js中引用hello.js

```js
var hello= require('./hello');

hello.world();
```

在上例中，hello.js通过exports对象把world作为模块的访问接口，在main.js中通过require('./hello')加载这个模块。然后就可以直接访问main.js中exports对象的成员函数了。

node.js四类模块优先级：

文件模块缓存区>>原生模块缓存区>>原生模块>>文件模块

## node.js常用工具util

util欧维node.js的一个核心模块，能够提供常用函数的集合，弥补核心JavaScript的功能国语精简的不足。

### util.inherits（继承）

util.inherits(constructor,superConstructor),后面是被继承的父类，前面是继承的子类

```js
var util = require('util');
function Base(){
    this.name='base';
    this.base=1991;
    this.sayHello = function(){
        console.log('hello'+this.name);
    };
}
 Base.prototype.showName= function(){
     console.log(this.name);
 };
 function Sub(){
     this.name='sub';
 }
 util.inherits(Sub,Base);
 var objBase= new Base();
 objBase.showName();
 objBase.sayHello();
 console.log(objBase);
 var objSub= new Sub();
 objSub.showName();
 //objSub.SayHello();
 console.log(objSub);


```

**注意：**Sub仅仅继承了Base在原型中定义的函数，而构造函数内部创造的base属性和sayHello函数都没有被Sub继承。

同时，在原型中定义的属性不会被console.log作为对象的属性输出。

### util.inspect（检查，视察）

### util.isArray(object)

如果给定的参数 "object" 是一个数组返回true，否则返回false。

```js
var util=require('util');

console.log(util.isArray([]));

console.log(util.isArray(new Array));

console.log(util.isArray({}));

```

true

true 

false

### util.isRegExp(object)

如果给定的参数"object"是一个正则表达式返回true，否则返回false。

### util.isDate(object)

如果给定的参数 "object" 是一个日期返回true，否则返回false。

```js
var util=require('util');

var b1 =util.isDate(new Date());

var b2= util.isDate(Date());

var b3= util.isDate({});

console.log("b1 is "+b1+"\nb2 is "+b2+"\nb3 is "+b3);
```

b1 is true
b2 is false
b3 is false

### util.isError(object)

如果给定的参数 "object" 是一个错误对象返回true，否则返回false。

## node.js文件系统

node.js文件系统被封装在fs模块中，他提供了文件的读取、写入、更名、删除、便利目录、连接等POSIX文件系统操作。







