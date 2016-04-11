# json-schema(一)

### 相关知识点
1. 它是什么
描述json的数据格式
2. 有什么优点
	+ 描述自定义的数据格式
	+ 清晰，对人和机器友好
	+ 完整的结构校验
		- 自动化测试
		- 校验表单提交数据 

### 一个简单的示例构建自己的json-schema	
1. 一个数据对象或者API的数据是这样的
```
{
    "id": 1,
    "name": "A green door",
    "price": 12.50,
    "tags": ["home", "green"]
}
```
2. 	json-scheme类似xml或者html，有一个声明的头
这里可以看到有文档遵循的协议格式，标题，描述以及文档的类型
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object"
}
```

3. 	如何描述对象的属性，以及对象属性上的规则

```
"properties": {
    "id": {
        "description": "The unique identifier for a product",
        "type": "integer"//数据的类型
    },
    "name": {
        "description": "Name of the product",
        "type": "string"
    },
   "price": {
       "type": "number",
       "minimum": 0,//最小值
       "exclusiveMinimum": true//排除掉最小值，不允许为0
   },
   "tags": {
       "type": "array",//数据的类型是数组
       "items": {//数组里每个item都是字符串
           "type": "string"
       },
       "minItems": 1,
       "uniqueItems": true
   }
},
"required": ["id", "name"]

```

### 实际应用

1. 数据校验

完成表单数据校验，数据类型，格式定义和实际dom分离
[schema](https://github.com/akidee/schema.js)

```
var validation = schema.validate({

    q: 'OK',
    start: -5,
    num: -100.99,
    xyz: 'additionalProperty'
})
```

未压缩情况下面加起来不到20kb，压缩体积会更小，适合数据校验
> 直出后这里node后台层应该是需要一个强类型的结构来定义协议，jsonschema是个不错的选择

2. 通过json-scheme生成表单，动态配置属性，json格式可以在外部定义，可以继承等等

[json-editor](http://jeremydorn.com/json-editor/)



3. jsonschema数据格式生成器

[jsonschema.net](http://jsonschema.net/#/)
使用者自己书写数据，框架根据数据生成格式

4. 文档格式生成器

[demo](http://mattyod.github.io/matic-draft4-example/)
目前仅支持jade语法。。。。

5. 数据格式的重用，继承
	+ 直接引用另一个jsonschema中定义的类型
	+ 通过运算符对引用的json格式做扩展，引入
    
相关文档可参考[这里](http://spacetelescope.github.io/understanding-json-schema/)

### 工具支持

方便书写jsonschema格式
[vm工具](https://github.com/Quramy/vison) 
[json schema lint](http://jsonschemalint.com/draft4/) 一个在线的格式检验工具，可以作为插件集成到构建中去
### 相关标准文档

1. [json-schema core](http://json-schema.org/latest/json-schema-core.html)
描述基本的json schema格式
2. [json-schema validate](http://json-schema.org/latest/json-schema-validation.html)
描述json-schema的校验标准

