# 学习笔记
	分编程语言，Javascript数据类型、对象 两部分内容
	前一部分内容主要是通过编程原理相关的内容，讲述词法、语法、类型、运行时等知识点
	但这两块内容在课程的设置上感觉衔接得很生硬

## 编程语言
### 语言按照语法分类：
	非形式语言：语法没有严格定义；
	形式语言（乔姆斯基谱系）：形式化定义；
		0型 无限制文法
		1型 上下文相关文法
		2型 上下文无关文法
		3型 正则文法
	后面的定义包含前面的定义

### 产生式（BNF）
	用尖括号括起来的名称来表示语法结构名
	语法结构分成基础结构和需要用其他语法结构定义的复合机构
		基础结构称终结符
		复合结构称非终结符
	引号和中间的字符表示终结符
	可以有括号
	\* 表示重复多次
	|表示或
	+表示至少一次
	
	BNF 四则运算
	<MultiplicativeExpression>::=<Number>|
		<MultiplicativeExpression>"*"<Number>|
		<MultiplicativeExpression>"/"<Number>|
	<AddtiveExpression>::=<MultiplicativeExpression>|
		<AddtiveExpression>"+"<MultiplicativeExpression>|
		<AddtiveExpression>"-"<MultiplicativeExpression>|

	四则运算（带括号）
	<Expression> ::= 
	    "("<AdditiveExpression>")"
	    |<AdditiveExpression>
	
	<AdditiveExpression> ::= 
	    <MultiplicativeExpression>
	    |<AdditiveExpression>"+"<MultiplicativeExpression>
	    |<AdditiveExpression>"-"<MultiplicativeExpression>
	
	
	<MultiplicativeExpression> ::= 
	    <Number>
	    |<MultiplicativeExpression>"*"<Number>
	    |<MultiplicativeExpression>"/"<Number>
		
		
### 现代语言的分类
	形式语言--用途
		数据描述语言（JSON）
		编程语言（Java，JavaScript）
	 
	形式语言--表达方式
		声明式语言
		命令型语言

	数据描述
		json\html\xml\sql\css
	编程语言
		c\c++\java\c#\python\javascript\rust
	声明式
		json\html\xml\sql\css
	命令式
		c\c++\java\c#\python\javascript\rust
		
### 图灵完备性 
	命令式--图灵机
		goto
		if 和 while
	盛明师--lambda
		递归
		
	动态
		在用户的设备/在线服务器上
		产品实际运行时
		Runtime
	静态
		在程序员的设备上
		产品开发时
		Compiletime

    类型系统
		动态类型系统与静态类型系统
		强类型与弱类型
		符合类型
		子类型
		泛型

### 一般命令式编程语言
	Atom、Expression、Statement、Structure、Program；


---------------------------------------------------------------
## JavaScript

### JS类型
	Number,Null,Undefined,Boolean,String,Symbol,BigInt,Object
	
#### Number
	IEEE 754 Double FLoat
	0.1 + 0.2 == 0.3 false
	1. toString()
		
#### String
	Character
	Code Point
	Encoding
	
	ASCII
	Unicode
	UCS
	GB
	ISO-8859
	BIG5

用UTF8对string 进行编码

	function UTF_Encoding(string) {
	  return Uint8Array.from(encodeURIComponent(string).replace(/%(..)/g, (m, v) = >{
	    return String.fromCodePoint(parseInt(v, 16))
	  }), c = >c.codePointAt(0))
	}
	

	参考资料：
	 http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html
	 https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder
	
#### Boolean
	值：true/false
	
#### Null
	表示有值但是为空

#### Undefined
	根本没有人设过这个值，没有定义；
	Null是关键字；Undefined不是关键字，
	
#### 对象
	基于类描述对象
	基于原型类描（JavaScript基于此方式）
	在JavaScript运行时，只需要关心原型和属性两个部分；

##### Object泛谈
	一个对象有三个性质
	
	identifier： 在编程语言里面一般用内存地址表示它的唯一性
	
	state：在编程语言中一般叫做属性
	
	behavior：在编程语言中一般叫做方法
	
##### 编程语言中有两大类抽象Object的方式
	
Class

	有归类和分类两种流派，归类呢就是说这个Object属于那个类别.

	所以从不同的角度来看，一个Object可能属于不同的多个类，所以多继承是很自然的一个现象了，c++就是属于归类的。

	分类的就是从最基本的基类往下分，Java采用这种方式，就没有多继承了。
	
Prototype

	原型的方式呢就是一个Object有什么原型属性，然后可以基于这个Object新建一个Object的时候，主要注重自己与原型的区别即可。
	
	最基本的原型是Nihilo，就是虚无，对应到JS就是Null.
	
	状态是Object中的核心，一个Object的本质就是一堆状态和能够改变这些状态的方法的集合。
	
##### Object in JS

	在JS中，因为func也可以赋值给变量，Object的state和behavior都统一到了property，所以只需要关心property和prototype。
	
	在JS中，当访问属性的时候的时候会溯源到自己基于原型的Object的属性，一直到基于的原型是Null了。所以也叫做原型链。
	
	我们使用kv的方式来访问Object的属性
	
	可以是string，也可以是Symbol类型，v可以是data，也可以是accessor
	
##### Object API

	Object.defineProperty
	
	在JS中，如果使用Prototype的方式抽象Object的话，就用
	
	Object.create
	Object.setPrototypeOf
	Obejct.getPrototypeOf
	如果使用Class的方式抽象Object
	
	··· new / class/ extends ···
	
JavaScript 狗咬人的代码

	class Human{
	  constructor(){
	    this.bloodValue = 100;
	  }
	
	  hurt(damage){
	    console.log(`啊~~~！~~~~~~~~~~${damage}`);
	    this.bloodValue--;
	  }
	}
	
	let human = new Human();
	human.hurt('狗咬人了');
	console.log(`此人血量仅剩：${human.bloodValue}`);

	
JavaScript 标准里面所有具有特殊行为的对象

	Object
	[[GetPrototypeOf]]
	[[SetPrototypeOf]]
	[[IsExtensible]]
	[[PreventExtensions]]
	[[GetOwnProperty]]
	[[DefineOwnProperty]]
	[[HasProperty]]
	[[Get]]
	[[Set]]
	[[Delete]]
	[[Enumerate]]
	[[OwnPropertyKeys]]
	
	Bound
	Array
	String
	Arguments
	Integer
	Module
	
	Function
	[[Call]]
	[[Construct]]
	
	Proxy 
	[[GetPrototypeOf]]
	[[SetPrototypeOf]]
	[[IsExtensible]]
	[[PreventExtensions]]
	[[GetOwnProperty]]
	[[DefineOwnProperty]]
	[[HasProperty]]
	[[Get]]
	[[Set]]
	[[Delete]]
	[[Enumerate]]
	[[OwnPropertyKeys]]
	[[Call]]
	[[Construct]]


	参考资料：
	https://www.w3.org/html/ig/zh/wiki/ES5/%E7%B1%BB%E5%9E%8B#Object.E7.9A.84.E5.86.85.E9.83.A8.E5.B1.9E.E6.80.A7.E5.8F.8A.E6.96.B9.E6.B3.95[https://www.w3.org/html/ig/zh/wiki/ES5/%E7%B1%BB%E5%9E%8B#Object.E7.9A.84.E5.86.85.E9.83.A8.E5.B1.9E.E6.80.A7.E5.8F.8A.E6.96.B9.E6.B3.95](https://www.w3.org/html/ig/zh/wiki/ES5/%E7%B1%BB%E5%9E%8B#Object.E7.9A.84.E5.86.85.E9.83.A8.E5.B1.9E.E6.80.A7.E5.8F.8A.E6.96.B9.E6.B3.95)

	[http://www.ecma-international.org/ecma-262/6.0/#sec-object-internal-methods-and-internal-slots](http://www.ecma-international.org/ecma-262/6.0/#sec-object-internal-methods-and-internal-slots)
	
 


  
 
