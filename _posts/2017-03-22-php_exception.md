
<!-- lang: html -->

# {{ page.title }}

### 前言
工作原因经常review其他同事的代码，总会发现一些coding的缺点，之前发现的一个普遍的问题就行很多人用其他语言的代码风格来写php，比如有个javaer在写php的时候总是用反射，有个c++er在写php的时候总是参数传一个array后面紧跟着一个参数表示array的大小，不知道让他写python的时候会不会也这样搞。今天发现一个有趣的问题就是有的人写代码从来不用exception，不用异常的话有些逻辑写起来就很诡异，比如层层的函数通过true和false表示执行成功和失败，在每次函数调用都加上判断是否调用成功，如果调用失败就把false层层传递出去，可以补脑那哥们在写代码时的小心翼翼，但是这样的代码真的很“拙劣”，这就是不用exception的结果，不知道大家在coding中是否会用的异常，今天整理一下php异常的知识。

### exception介绍
php中Exception是所有异常的基类
属性：
message
异常消息内容
code
异常代码
file
抛出异常的文件名
line
抛出异常在该文件中的行号

方法：
Exception::__construct — 异常构造函数
Exception::getMessage — 获取异常消息内容
Exception::getPrevious — 返回异常链中的前一个异常
Exception::getCode — 获取异常代码
Exception::getFile — 创建异常时的程序文件名称
Exception::getLine — 获取创建的异常所在文件中的行号
Exception::getTrace — 获取异常追踪信息
Exception::getTraceAsString — 获取字符串类型的异常追踪信息
Exception::__toString — 将异常对象转换为字符串
Exception::__clone — 异常克隆

### exception简单实用
``` php
try { 
	$error = 'my error!'; 
	throw new Exception($error) 
} catch (Exception $e) { 
	echo $e->getMessage(); 
} 
```
php中Exception的基本使用方式就是这样，通过try，catch语句实现异常的捕获，在catch中实现异常的处理方式，需要注意的是在php中，异常必须手动抛出，当捕获到一个异常后，try()块里面的后续代码将不会继续执行，而是会尝试查找匹配的“catch”代码块 
当抛出一个异常后，如果不进行catch处理，则会报“Uncaught exception 'Exception'”错误；catch可以有多个，用来处理不同类型的异常，会匹配到第一个相符合的类型，如果把基类作为第一个catch的类型，后面所有的类型都不会起作用了。

### 自定义exception
只要继承Exception就可以实现不同类型的异常类型，可以对不同类型的异常进行不同的处理。下面是一个简单的例子
``` php
class MyException extends Exception 
{ 
	public function __construct($message, $code = 0) { 
		parent::__construct($message, $code); 
	} 
	public function __toString() { 
		return __CLASS__ . ": [{$this->code}]: {$this->message}\n"; 
	} 
	public function customFunction() { 
		echo "A Custom function for this type of exception\n"; 
	} 
} 
```
### 结语
这就是我知道的php的异常的所有知识了，希望在适合的场景下使用异常

{{ page.date|date_to_string }}
