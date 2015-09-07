## thinkjs

### 开始
+  安装
npm install -g thinkjs-cmd
+  查看是否安装成功
thinkjs -v
+ 新建项目
mkdir new_dir_name; 
cd new_dir_name;
thinkjs . //正常到这里会自动打开浏览器页面访问项目
+ 手动启动
node www\index.js

### 配置
thinkjs的配置有很多，`系统默认配置 -> 应用配置 -> 调试配置 -> 模式配置`
基本上只用到应用配置，应用配置的路径是`App/Conf/config.js`，
配置在程序中是很方便读取和写入的
```
//读取
var dbHost = C('db_host');
//写入
C('name', 'xxx');
C({
    'name': 'welefen',
    'use_cluster': false
})
```
>系统默认配置路径：`lib/Conf/config.js`
>调试配置路径：`App/Conf/debug.js`
>模式配置路径：`App/Conf/mode.js`


### 架构
#### url中的含义
一个典型的url：`http://hostname:port/分组/控制器/操作/参数名/参数值/参数名2/参数值2?arg1=argv1&arg2=argv2`

`分组` 一个应用下有多个分组，一个分组都是很独立的模块。比如：前台模块、用户模块、管理员模块
`控制器`一个分组下有多个控制器，一个控制器是多个操作的集合。如：商品的增删改查
`操作` 一个控制器有多个操作，每个操作都是最小的执行单元。如：添加一个商品

#### CBD模式
CBD模式，核心Core+行为Behavior+驱动Driver

##### 核心(Core)
thinkjs的核心部分包含通用函数库、系统默认配置、核心类库等组成，这些都是thinkjs必不可少的部分。
```
lib/Common/common.js 通用函数库
lib/Common/extend.js js原生对象的扩展
lib/Common/function.js 框架相关的函数库
lib/Conf/alias.js  系统类库别名，加载时使用
lib/Conf/config.js 系统默认配置
lib/Conf/debug.js debug模式下的配置
lib/Conf/mode.js  不同模式下的配置
lib/Conf/tag.js  每个切面下的行为
lib/Lib/Core/App.js  应用核心库
lib/Lib/Core/Controller.js 控制器基类
lib/Lib/Core/Db.js  数据库基类
lib/Lib/Core/Dispatcher.js 路由分发类
lib/Lib/Core/Http.js 封装的http对象类
lib/Lib/Core/Model.js 模型基类
lib/Lib/Core/Think.js 框架类
lib/Lib/Core/View.js 视图类
lib/Lib/Util/Behavior.js 行为基类
lib/Lib/Util/Cache.js 缓存基类
lib/Lib/Util/Cookie.js cookie类
lib/Lib/Util/Filter.js 数据过滤类
lib/Lib/Util/Session.js session基类
lib/Lib/Util/Valid.js 验证类

```

##### 行为(Behavior)
行为是扩展机制里面特殊的扩展，它定义的是一个行为或者事件。
标签是一组行为的集合，行为按顺序执行。
系统的标签定义：

- `app_init` 应用初始化
- `path_info` 解析path路径
- `resource_check` 静态资源请求检测
- `route_check` 路由检测
- `app_begin` 应用开始
- `action_init` action初始化
- `view_init` 视图初始化
- `view_template` 模版定位
- `view_parse` 模版解析
- `view_filter` 模版内容过滤
- `view_end` 视图结束
- `action_end` action结束
- `app_end` 应用结束

系统配置如下（也可以在`App/Conf/tag.js`中自定义标签 ）：
```
/**
 * 系统标签配置
 * 可以在App/Conf/tag.js里进行修改
 * @type {Object}
 */
module.exports = {
    //应用初始化
    app_init: [],
    //pathinfo解析
    path_info: [],
    //静态资源请求检测
    resource_check: ['CheckResource', 'CheckResourse2'],
}
```

#### 驱动(Driver)

除了核心和行为外，thinkjs里的很多功能都是通过驱动来实现的，如：Cache, Session, Db等。

驱动包括：

`lib/Lib/Driver/Cache` 缓存驱动
`lib/Lib/Driver/Db` 数据库驱动
`lib/Lib/Driver/Session` Session驱动
`lib/Lib/Driver/Socket` Socket驱动
`lib/Lib/Driver/Template` 模版引擎驱动
如果有些功能框架里还没实现，如：mssql数据库，那么开发人员可以在项目里 App/Lib/Driver/Db/ 里实现。

#### 自动加载
这里有thinkjs框架里面的文件，可以使用thinkRequire加载，非thinkjs里面的文件内部仍然是使用系统的require来加载
可以快速加载的`xxxBehavior`, `xxxModel`,` xxxController`, `xxxCache`, `xxxDb`, `xxxTemplate`, `xxxSocket`, `xxxSession`


### 路由
路由定义了前端url到后端路径或者服务的一组映射规则，当访问`http://hostname:port/分组/控制器/操作/参数名/参数值/参数名2/参数值2?arg1=argv1&arg2=argv2`时，通过`url.parse`解析到的`pathname为/分组/控制器/操作/参数名/参数值/参数名2/参数值2`。

#### url过滤
正常使用的url可能不是上面的`/分组/控制器/操作/`规则，可能有一些前缀或者后缀，通过如下配置可以修改。
```
//url过滤配置
'url_pathname_prefix': '', //前缀过滤配置
'url_pathname_suffix': '.html' //后缀过滤配置
```
#### 路由识别
通过上一步的url过滤之后，即可以按照`/分组/控制器/操作/`的方式来分割参数了，如果第一个路净值在分组列表中不存在，则第一个路径的值是分配到控制器上面。
```
//分组列表
'app_group_list': ["Home", "Admin"],
'default_group': 'Home', //默认分组
```
例：pathname为：/admin/group/list，会加载App/Lib/Controller/Admin/GroupController.js文件，实例化该类，并调用listAction方法。
#### 自定义路由
可以通过自定义规则来配置，路由配置文件为：`App/Conf/route.js`
```
//自定义路由规则
module.exports = [
    ["规则1", "需要识别成的path"], //规则到具体pathname的映射
    ["规则2", {  //同一个规则根据不同的请求方式对应到不同的pathname上
        "get": "get请求时识别成的path", 
        "delete": "delete请求时识别成的path",
        "put,post": "put,post请求时识别成的path" //请求方式为put或者post时对应的pathname
    }]
]
```

### 控制器
控制器是分组下一类功能的集合，每个控制器是一个独立的类文件，每个控制器下有多个操作。

#### 定义控制器
创建文件App/Lib/Controller/Home/ArticleController.js，表示Home分组下有名为Article的控制器。文件内容如下：
```
//控制器文件定义
module.exports = Controller(function(){
    return {
        indexAction: function(){
            return this.diplay();
        },
        listAction: function(){
            return this.display();
        },
        detailAction: function(){
            var doc = this.get("doc");
        }
    }
});
```
这里的控制器采用的是继承的方式，一些公用的操作可以写在基类中，非常方便。
>比如登录场景下的校验、获取导航的分类信息、常规的容错处理

### 模型
这里主要用来完成和持久层的数据库操作，如增删查改。

#### 定义一个模型
这里也集成的是系统的model
```
//模型定义
module.exports = Model(function(){
    return {
        //获取用户列表
        getList: function(){

        }
    }
})
```

####  模型和数据表的映射
模型的名称也是按驼峰约束，也是集成自系统的Model类。
如模型名称是UserModel，对应的表名是user（假设没有表前缀）

#### 实例化
```
//第一种
//通过thinkRequire加载UserModel对应的js文件
var userModel = thinkRequire("UserModel");
var instance = userModel();
//传递参数实例化
var newInstance = userModel("users", {db_prefix: ""});

//第二种
//实例化模型类
var model = D('User');
```

#### 支持链式操作
```
var promise = D('User').where({name: 'user'}).field('sex,date').find().then(function(data){
    //data为当前查询到用户信息，如果没有匹配到相关的数据，那么data为一个空对象
})
```

### 小结
了解了基本的使用以及核心的一些概念，这些内容官网文档上也有的，这里自己整理下，加深下印象。说说对thinkjs的一些看法吧

+ 基于promise，有一套自己的规范。基于规范开发代码，代码会很清晰（按规范来写才好，如果随意发挥代码看着还是很痛苦的）
+ 有一整套自己的解决方案，方便快速搭建系统
+ 数据库的crud封装的很好，便于操作，使数据库底层操作对开发者透明，api很友好，可惜当前只支持mysql和mongodb
+ 开发流程和模式是固定了的，是一个轻量级的框架，有特定的应用场景。譬如想快速构建一个小型web应用

总的来说，还是比较好用的
