# roadmap
My roadmap as a SDE

## PHP

### 原理

* 源码结构
	* build: 放置一些和源码编译相关的一些文件
	* ext: 官方扩展，包括了绝大多数PHP的函数的定义和实现。个人写的扩展在测试时也可以放到这个目录，方便测试和调试
	* main: 实现PHP的基本设施，**这里和Zend引擎不一样**
	* Zend: Zend引擎的实现目录
	* pear: PHP扩展与应用仓库，包含PEAR的核心文件
	* sapi: 包含了各种服务器抽象层的代码
	* TSRM(Thread Safe Resource Manager): 线程安全资源管理器
	* tests: PHP的测试脚本集合
	* win32: 这个目录主要包括Windows平台相关的一些实现
	* [参考](http://www.php-internals.com/book/?p=chapt01/01-02-code-structure)

* 全局变量宏
	* PG()，EG(): 解决线程安全所写的全局变量包裹宏
	* \_php\_core\_globals: 字段很大一部分是 **与php.ini文件中的配置项对应的** 。在PHP启动并读取php.ini文件时就会对这些字段进行赋值， 而用户空间的ini\_get()及ini\_set()函数操作的一些配置也是对这个全局变量进行操作的。
	* [参考](http://www.php-internals.com/book/?p=chapt01/01-03-comm-code-in-php-src)	

* 用户代码执行
	* PHP本身实现了把用户的逻辑“翻译”为机器语言来执行的功能
	* 词法分析、语法分析: 
		* 词法分析是把PHP代码分割成一个个的“单元”（TOKEN）
		* 语法分析则将这些“单元”转化为Zend Engine可执行的操作
	*  Zend Engine: 负责最终操作的执行和结果的返回

* PHP代码生命周期
	* SAPI: PHP解释器用于接收输入的接口，面向不同的Web Server和命令行
	* 开始阶段: 在处理请求之前。
		* 模块初始化(MINIT)：只执行一次(例如Apache启动时)，回调所有模块的MINIT函数。
		
			```
			PHP_MINIT_FUNCTION(myphpextension)
			{
				// 注册常量或者类等初始化操作
				return SUCCESS; 
			}
			``` 
		* 模块激活(RINIT)：发生在请求阶段，每次请求之前执行。

			```
			PHP_RINIT_FUNCTION(myphpextension)			{				// 例如记录请求开始时间				// 随后在请求结束的时候记录结束时间。这样我们就能够记录下处理请求所花费的时间了				return SUCCESS; 			}
			```
	* 结束阶段: 脚本执行到结尾，或者调用 ``exit()`` 或 ``die()``。RSHUTDOWN 对应 RINIT，MSHUTDOWN 对应 MINIT。

		```
		PHP_RSHUTDOWN_FUNCTION(myphpextension)		{			// 例如记录请求结束时间，并把相应的信息写入到日至文件中。			return SUCCESS; 		}
		```
	* CLI/CGI模式的PHP属于单进程的SAPI模式
		* MINIT->RINIT->执行请求->RSHUTDOWN->MSHUTDOWN
	* 解析php.ini: 由 php\_init\_config 负责，主要是查找ini文件
	* 全局操作函数的初始化: 由 php\_startup\_auto\_globals 负责，初始化$_GET等常量
	* 当一个PHP文件需要解析执行时，它可能会需要执行三个文件，其中包括一个前置执行文件、当前需要执行的主文件和一个后置执行文件。
		* 在php.ini文件通过auto\_prepend\_file参数和auto\_append\_file参数设置。 
	* PHP扩展大都使用Zend API，PHP核心开发者提出要将这种耦合解开
	
* SAPI概述
	* 在各个服务器抽象层之间遵守着相同的约定，这里我们称之为SAPI接口
	* 每个SAPI实现都是一个\_sapi\_module\_struct结构体变量


### 脚本执行
* PHP的词法分析器是通过lex生成的，词法规则文件在$PHP\_SRC/Zend/zend\_language\_scanner.l
	* token\_get\_all，查看词法分析结果
* PHP使用bison生成语法分析器，规则见$PHP\_SRC/Zend/zend\_language\_parser.y
* Lex和Yacc是Unix下的两个文本处理工具，主要用于编写编译器，也可以做其他用途。
	* php使用re2c和bison

### opcode



(待续...)