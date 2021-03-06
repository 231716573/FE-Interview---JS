## 典型常见JS：
***

#### 1.用JS将字符串 '[{"a":"1", "b":"2", "c":"3"}]' 转为数组，然后弹出a下面的1.

答案1：
```javascript
var str = "[{'a':'1', 'b':'2' , 'c': '3'}]";
str = eval("("+str+")")
console.log(str[0].a)
```
      

#### 2. a++ 与 ++a 区别
```javascript
var a=10, b=20, c=30;
++a;  // 11
a++;  // 11
e = ++a+(++b)+(c++)+a++; // 13+(21)+(30)+13;
console.log(e);  //77
```

#### 3.变量提升以及函数作用域
  ##### 3.1
```javascript
f1()
console.log("."+a); // 报错，提示 a is not defined
console.log("."+b);
console.log("."+c);
function f1(){
    var a=b=c=9;
    console.log(a); // 9
    console.log(b); // 9
    console.log(c); // 9
}
```
  ##### 3.2
```javascript
var a = 18;
f1();
function f1(){
    var b = 9;
    console.log(a);  // undefined
    console.log(b);  // 9;
    var a = '123';
}
```

#### 4.题目：填写内容让下面代码支持a.name = “name1”; b.name = “name2”;
```javascript
function obj(name){
    _______
}
obj._____ = "name2";
var a = obj("name1");
var b = new obj;
```

答案：//创建全局函数
```javascript
function obj(name){
     if(name) {        // 区分普通调用和实例化调用
        this.name = name;
      }
      return this;    // 返回this引用，调用时this指向window
}

obj.prototype.name = "name2";    // 设置原型对象
var a = obj("name1");    //直接调用函数，a等于window，name为window的属性。
var b = new obj;    //调用函数实例化对象，this指向obj的实例化对象。
```
    
#### 5.

1、person1.__proto__ 是什么？<br>
2、Person.__proto__ 是什么？<br>
3、Person.prototype.__proto__ 是什么？<br>
4、Object.__proto__ 是什么？<br>
5、Object.prototype__proto__ 是什么？<br>

答案：
##### 第一题：
因为 person1.__proto__ === person1 的构造函数.prototype <br>
因为 person1的构造函数 === Person<br>
所以 
```javascript
person1.__proto__ === Person.prototype
```

##### 第二题：
因为 Person.__proto__ === Person的构造函数.prototype<br>
因为 Person的构造函数 === Function<br>
所以 <br>
```javascript
Person.__proto__ === Function.prototype
```

##### 第三题：
Person.prototype 是一个普通对象，我们无需关注它有哪些属性，只要记住它是一个普通对象。<br>
因为一个普通对象的构造函数 === Object<br>
所以 
```javascript
Person.prototype.__proto__ === Object.prototype
```

##### 第四题，参照第二题，因为 Person 和 Object 一样都是构造函数
```javascript
Object.__proto__ == Function.prototype
```
##### 第五题：
Object.prototype 对象也有proto属性，但它比较特殊，为 null 。因为 null 处于原型链的顶端，这个只能记住。
```javascript
Object.prototype.__proto__ === null
```
##### 原型链例子：
```javascript
function Person(){}
var person1 = new Person();
console.log(person1.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype) //true
console.log(Object.prototype.__proto__) //null

Person.__proto__ == Function.prototype; //true
console.log(Function.prototype)// function(){} (空函数)

var num = new Array()
console.log(num.__proto__ == Array.prototype) // true
console.log( Array.prototype.__proto__ == Object.prototype) // true
console.log(Array.prototype) // [] (空数组)
console.log(Object.prototype.__proto__) //null

console.log(Array.__proto__ == Function.prototype)// true
```

##### 疑点解惑：
```javascript
1、Object.__proto__ === Function.prototype // true
```
Object 是函数对象，是通过new Function()创建的，所以Object.__proto__指向Function.prototype。（参照第八小节：「所有函数对象的__proto__都指向Function.prototype」）
```javascript
2、Function.__proto__ === Function.prototype // true
```
Function 也是对象函数，也是通过new Function()创建，所以Function.__proto__指向Function.prototype。
```javascript
3、Function.prototype.__proto__ === Object.prototype //true
```
其实这一点我也有点困惑，不过也可以试着解释一下。<br>
Function.prototype是个函数对象，理论上他的__proto__应该指向 Function.prototype，就是他自己，自己指向自己，没有意义。<br>
JS一直强调万物皆对象，函数对象也是对象，给他认个祖宗，指向Object.prototype。Object.prototype.__proto__ === null，保证原型链能够正常结束。

#### 6.建立一个function,接受参数n=5,不用for循环输出数组【1,2,3,4,5】
```javascript
function show(n) {
    var arr = [];
    return (function fn() {
        arr.unshift(n);
        n--;
        if (n != 0) {
            fn();
        }
        return arr;

    })()
}
show(5)//[1,2,3,4,5]
```

#### 7.判断类型
```javascript
console.log(typeof(null))        //object
console.log(typeof(undefined))   //undefined
console.log(typeof(Number))      //function
console.log(typeof(String))      //function
console.log(typeof(NaN))         //number
console.log(typeof(object))      //undefined
var str = 'abc'
console.log(typeof(str++))       //number
console.log(str)                 //NaN
console.log(NaN==undefined);     //false
console.log(NaN==NaN);           //false
```
#### 8.输出y跟z
```javascript
var x = 1, y=z=0;
function add(n){
	n = n + 1;
}
y = add(x);  //undefined
function add(n){
	n = n + 3;
}
z = add(x); //undefined
s = y + z;  //NaN
```
我们首先看function add，两个add都没有返回值，而我们知道，没有明确返回值的，全部返回undefined，<br>
所以，y和z都会是undefined，那么s自然也就不会是一个数字了，没错，s应该是NaN

#### 9.
```javascript
function Foo() {
    var i = 0;
    return function() {
        console.log(i++);
    }
}
 
var f1 = Foo(),
    f2 = Foo();
f1();
f1();
f2();

//0 1 0
```
#### 10.优化字符串拼接
```javascript
var str="我喜欢我可爱的女朋友，"；
str=str+"她叫喵喵，";
str=str+"她时而可爱，时而认真，";
str=str+"她那天真的笑声可以让人忘掉一切烦恼。";
console.log(str);
```
答案：
这里的优化主要是对加号操作符的优化，因为加号在JavaScript中非常耗时和耗内存，需要经过以下六步：

1、首先开辟一块临时空间，存储字符串， <br>
2、然后在开辟一块空间 <br>
3、把str中的字符串复制到刚刚开辟的空间 <br>
4、在把需要连接的字符串复制到str后面 <br>
5、str指向这块空间 <br>
6、回收str原来的空间和临时空间 <br>
优化的方法是使用数组的push方法，数组是连续的存储空间，可以省下很多步

```javascript
var res=[];
var str="我喜欢我可爱的女朋友，"；
res.push(str);
res.push("她叫喵喵，");
res.push("她时而可爱，时而认真，");
res.push("她那天真的笑声可以让人忘掉一切烦恼。");
console.log(res.join("")); 
```
#### 11. [1,2,3].map(parseInt)
答案：
```javascript
[1,NaN,NaN]  //因为parseInt(1,0) = 1,parseInt(2,1) = NaN,parseInt(3,2) = NaN
```
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19].map(parseInt)
答案：
```javascript
[1, NaN, NaN, NaN, NaN, NaN, NaN, NaN, NaN, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27]  //因为parseInt(10,9)=9
```

#### 12.输出z等于什么
```javascript
var z = 10;
function foo(){
	console.log(z); 
}
(function (funArg){
	var z = 20;
	funArg()
})(foo);

//答案：10
```



#### 13.
```javascript
function myFunction(a,b){
	if(a > b){
		return;
	}
	x = a + b;
}
myFunction(5,4);
console.log(x); // x is not defined

myFunction(4,5);
console.log(x); // 9
```