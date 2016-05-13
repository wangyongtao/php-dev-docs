


PHP框架互操作组(PHP Framework Interop Group,即PHP标准组)发布了一系列推荐风格。 
其中有部分是关于代码风格的，即PSR-0，PSR-1，PSR-2和PSR-4。 

可以使用PHP_CodeSniffer来检查代码是否符合这些标准，文本编辑器插件Sublime Text 2还能 提供实时检查。
如果不符合规范，可以使用Fabien Potencier提供的工 具PHP Coding Standards Fixer自动修复，不用自己手工修复。

PHP_CodeSniffer： http://pear.php.net/package/PHP_CodeSniffer/  
PHP Coding Standards Fixer： http://cs.sensiolabs.org/  

PSR-0: Autoloading Standard 自动加载规范  
PSR-1: Basic Coding Standard 基本代码规范  
PSR-2: Coding Style Guide 代码样式  
PSR-3: Logger Interface 日志接口  
PSR-4: Autoloader 自动加载器 


PSR原本有四个规范，分别是：PSR-0，PSR-1，PSR-2，PSR-3。
2013年底，新出了第5个规范——PSR-4(自动加载)，相当于PSR-0的升级版。



PSR-1: Basic Coding Standard

PHP代码文件必须以 <?php 或 <?= 标签开始；
PHP代码文件必须以 不带BOM的 UTF-8 编码；
PHP代码中应该只定义类、函数、常量等声明，或其他会产生 从属效应 的操作（如：生成文件输出以及修改.ini配置文件等），二者只能选其一；
命名空间以及类必须符合 PSR 的自动加载规范：PSR-0 或 PSR-4 中的一个；
类的命名必须遵循 StudlyCaps 大写开头的驼峰命名规范；
类中的常量所有字母都必须大写，单词间用下划线分隔；
方法名称必须符合 camelCase 式的小写开头驼峰命名规范。

英文版: http://www.php-fig.org/psr/psr-1/  
中文版：https://segmentfault.com/a/1190000002521577  


https://github.com/hfcorriez/fig-standards