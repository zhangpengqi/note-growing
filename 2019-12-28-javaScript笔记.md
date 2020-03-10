---
title: js文档笔记
categories: learn
date: 2019-12-28
---

## 函数

### 使用arguments对象

```js
function myConcat(separator){
    var result='';
    var i;
    for(i=1;i<arguments.length;i++){
        result+=arguments[i]+separator;
    }
    return result;
}
```

运行：

```js
// returns "red, orange, blue, "
myConcat(", ", "red", "orange", "blue");

// returns "elephant; giraffe; lion; cheetah; "
myConcat("; ", "elephant", "giraffe", "lion", "cheetah");

// returns "sage. basil. oregano. pepper. parsley. "
myConcat(". ", "sage", "basil", "oregano", "pepper", "parsley");
```

### 箭头函数

有两个因素会引入箭头函数：更简介的函数和this.

#### 更简洁的函数

举例：

```js
var a = [
  "Hydrogen",
  "Helium",
  "Lithium",
  "Beryllium"
];

var a2 = a.map(function(s){ return s.length });

console.log(a2); // logs [ 8, 6, 7, 9 ]

var a3 = a.map( s => s.length );

console.log(a3); // logs [ 8, 6, 7, 9 ]
```

#### this的词法

不用箭头函数的如下：

```js
function Person() {
  // The Person()构造函数定义this作为它自己
  this.age = 0;

  setInterval(function growUp() {
    //在非严格模式下，growUp()函数定义“this”
    //作为全局对象，它不同于“this”
    //由Person()构造函数定义。
    this.age++;
  }, 1000);
}

var p = new Person();
```

使用箭头函数后：

```js
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the person object
  }, 1000);
}

var p = new Person();
```

## 表达式和运算符

### 解构

```js
var foo = ["one", "two", "three"];

// 不使用解构
var one   = foo[0];
var two   = foo[1];
var three = foo[2];

// 使用解构
var [one, two, three] = foo;
```

## 索引集合类

### 数组方法

1、concat()  连接两个数组，并返回一个新数组

```js
var myArray=new Array("1","2","3");

myArray = myArray.concat("a","b","c");

//myArray is now ["1","2","3","a","b","c"]
```

２、join()　将数组的所有原始连接处一个字符串

```js
var myArray=new Array("Wind","Rain","Fire");

var list= myArray.join("-");//list is "Wind-Rain-Fire"
```

## js创建对象的方式

### 1、使用构造函数

用构造函数可以创建特定类型的对象，同事也可以创建自定义的构造函数，从而自定义对象类型的属性和方法。

```js
function Person(name,age){
    this.name=name;
    this.age=age;
    this.sayName=function(){
        alert(this.name);
    }
};



var person1=new Person("zhangfei",15);

console.log(person1);
```

运行结果：

1. *Person {name: "zhangfei", age: 15, sayName: \*ƒ*}*

2. 1. age: 15
   2. name: "zhangfei"
   3. sayName: *ƒ ()*
   4. __proto__: Object

### ２、使用对象初始化器

```js
var myHonda = {color: "red", wheels: 4, engine: {cylinders: 4, size: 2.2}};
```

### ３、使用 Object.create 方法

对象也可以用 [`Object.create()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create) 方法创建。该方法非常有用，因为它允许你为创建的对象选择其原型对象，而不用定义一个构造函数。

```js
// Animal properties and method encapsulation
var Animal = {
  type: "Invertebrates", // Default value of properties
  displayType : function() {  // Method which will display type of Animal
    console.log(this.type);
  }
}

// Create new animal type called animal1 
var animal1 = Object.create(Animal);
animal1.displayType(); // Output:Invertebrates

// Create new animal type called Fishes
var fish = Object.create(Animal);
fish.type = "Fishes";
fish.displayType(); // Output:Fishes
```

### ４、工厂模式

```js
function createPerson(){

​	var o=new Object();

​	o.name=name;

​	o.sex=sex;

​	o.sayName=function(){

​		alert(this.name);

​	}

​	return o;

}

var person1 =new createPerson('zhangfei','男');

var person2=new createPerson('guangyu','男');
```

### 子类继承

步骤：

１、定义父类Employee构造函数，在该构造函数内定义name,dept属性；

２、定义子类Manager构造函数，在该构造函数内调用Employee构造函数，并定义reports属性；

3、将一个获得了Employee.prototype(Employee构造函数原型)的新对象赋予manager构造函数，以作为manager构造函数的原型。

之后当你创建新的Manager对象实例时，该实例会从Employee对象继承name,dept属性。

示例：

```
//1、定义父类构造函数，定义属性

function Employee(){
    this.name="";
    this.dept="general";
}

//2、定义子类构造函数，在该构造函数内调用父类构造函数，定义新属性

function Manager(){

​	Employee.call(this);

​	this.reports=[];

}

//3、将一个获得了Employee.prototype(Employee构造函数原型)的新对象赋予manager构造函数，以作为manager构造函数的原型。

Manager.prototype=Object.create(Employee.prototype);
```

