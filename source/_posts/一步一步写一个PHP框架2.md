title: "一步一步写一个PHP框架（二）"
date: 2015-04-22 15:31:44
category: [web开发]
tags: [PHP,PHP框架]
---

在写第一个PHP框架的时候参考[这位大哥](!http://www.yuansir-web.com/2012/01/10/%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%E5%86%99php-mvc%E6%A1%86%E6%9E%B6%E4%B8%80/),的内容和Codeigniter框架的源码。

##程序框架的搭建

前提：已经搭建好了一个apache+PHP的开发环境，我使用的是Apache+mod_php5的方式。

在根目录`simplemvc`下新建以下文件夹

- `config` 用来存放配置文件
- `controller`用来存放控制器
- `lib`用来存放引入的库文件
- `model`用来存放模型
- `system`用来存放系统文件
	- `core`核心文件，包括核心controller控制器等，所有的controller都要继承于他
	- `lib`存放核心的库文件，包括route等
	- `app.php`应用程序驱动类
- `view`用来存放视图
- `index.php`项目的入口文件，程序是从这里开始执行的。

##定义系统路径

```
//预定义
define('SYSTEM_PATH', dirname(__FILE__).'/system');
define('ROOT_PATH',  substr(SYSTEM_PATH, 0,-7));
define('SYS_LIB_PATH', SYSTEM_PATH.'/lib');
define('APP_LIB_PATH', ROOT_PATH.'/lib');
define('SYS_CORE_PATH', SYSTEM_PATH.'/core');
define('CONTROLLER_PATH', ROOT_PATH.'/controller');
define('MODEL_PATH', ROOT_PATH.'/model');
define('VIEW_PATH', ROOT_PATH.'/view');
```
预定义一些常量，在之后的程序中我们会使用这些路径。

##加载配置文件，和应用程序驱动类
###配置文件
在config文件夹下的`config.php`
··
$CONFIG['system']['db']