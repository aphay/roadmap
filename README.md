# roadmap
My roadmap as a SDE

##PHP
###源码
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



(待续...)