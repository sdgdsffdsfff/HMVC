## 欢迎大家使用 CodeIgniter HMVC 扩展！


很显然，Codeigniter并不支持HMVC,何为HMVC,就是它把MVC根据模块又细化成多个MVC，看如下结构图你就明白了，原来Codeigniter的MVC结构图是：
![1](http://www.phpddt.com/usr/uploads/2013/12/1732703494.png)
让Codeigniter支持HMVC架构
实现HMVC后结构图为：
![2](http://www.phpddt.com/usr/uploads/2013/12/3639030302.png)
让Codeigniter支持HMVC架构
HEX为CodeIgniter做了HMVC扩展（https://github.com/CodeIgniter/HMVC）



如果您还不了解什么是 HMVC，请先移步维基百科查看：

[http://zh.wikipedia.org/wiki/HMVC](http://zh.wikipedia.org/wiki/HMVC)


一直感觉 CodeIgniter 缺乏好的 HMVC 架构，而且我个人认为目前的几个 HMVC 第三方类库都不是很好，有的要修改 CI 的源代码，有的要引入新的语法，这都不是我喜欢的方式，所以我自己研究了一个方案，希望大家多多提出宝贵意见。我的 HMVC 特点就是不修改 CI 源代码，不引入新的语法，完全是利用 CI 强大的扩展机制。

目前的扩展方式是在 application 目录下增加 modules 目录，每个模块有自己的目录，并且模块可以有一级子目录，比如 `application/modules/目录/模块名/....`

每个模块都有自己的 MVC 结构，像这样 `application/modules/模块名/controllers`，`application/modules/模块名/models`，`application/modules/模块名/views` 在视图中装载模块：

	$this->load->module('模块名/控制器/方法');

这里也可以使用 URL 路由中的默认控制器，默认的方法是 index() 方法，和普通控制器保持一致。如果要传递参数：

	$this->load->module('模块名/控制器/方法', array('参数1', '参数2', ...));

如果需要返回模块的结果而不想输出到屏幕，可以把第 3 个参数设置为 TRUE：

	$this->load->module('模块名', array('参数1', '参数2', ...), TRUE);

如果需要从 URL 访问某个模块的某个方法，URL 规则是这样的：

	http://domain/index.php/module/模块名/控制器/方法

实际上 `/module` 后面的内容和前面传入 `$this->load->module()` 中的参数一致。

如果要通过 URL 传递参数，则直接加在 URL 后面：

	http://domain/index.php/module/模块名/控制器/方法/参数1/参数2/..../参数n

另外，这里的 URI 可以使用路由规则，也就是说什么样的 URL 都可以，只要最后路由成符合上面的规则即可，比如要使用这样的 URL：

	http://domain/index.php/m/模块名/控制器/方法

可以在 routers.php 里添加一个路由规则：

	$route['m/(:any)'] = 'module/$1';

如果要在某个模块的视图里生成访问当前模块当前控制器的某方法的 URL，可以在视图里这样写：

	<?php echo $this->module_url('要访问的方法名/参数1/..../参数n'); ?>

如果要生成当前模块其他控制器的方法的 URL，可以这样：

	<?php echo $this->module_url('要访问的方法名/参数1/..../参数n', '控制器名'); ?>

基本上就是这样，如果大家有不清楚的，我会在论坛详细解答：

[http://codeigniter.org.cn/forums/thread-1319-1-1.html](http://codeigniter.org.cn/forums/thread-1319-1-1.html)

压缩包解压后，其中有控制器、模型、视图和模块的简单例子，并且其中只包含模块所需的代码，不包含 CI 核心代码。


### 更新记录

- 2013.4.18 修复一个在模块中的模型，无法访问当前模块变量的 BUG。
- 2012.4.8 修复一个自动装载类库后，模块中此类库无法使用的 BUG。
- 2012.2.19 增加对 CodeIgniter 2.1.0 的支持。
- 2011.8.9 修复从 URL 访问 Module 的时候，autoload 无效的 BUG。
- 2011.7.28 增加从 URL 访问 Module 的功能。
- 2011.4.13 修正 autoload 对 module 无效的 BUG。
- 2011.4.11 支持最新的 CI 2.0.0，完全为 PHP5 重写 HMVC 所有代码。
- 2011.1.8 支持在控制器里直接载入一个或多个模块；修复在模块里装载类库报错的 BUG；
- 2010.12.15 支持在控制器中直接装载模块。
- 2010.8.7 修正一个在 Module 的构造函数中装载 Model 报错的 BUG。
