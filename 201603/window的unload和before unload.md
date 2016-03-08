# window的unload和beforeunload

## unload
当文档或其子资源被卸载时，unload事件会被触发

文档会处于一个特殊状态:

+ 所有资源仍存在 (图片, iframe 等.)
+ 对于终端用户所有资源均不可见
+ 界面交互无效 (window.open, alert, confirm 等.)
+ 错误不会停止卸载文档的过程

```
 window.addEventListener('unload', function(event) {
   console.log('I am the 4th and last one…');
 });
```

[mdn: unload](https://developer.mozilla.org/zh-CN/docs/Web/Events/unload)

## beforeunload
当浏览器窗口，文档或其资源将要卸载时，会触发beforeunload事件。

如果处理函数为Event对象的returnValue属性赋值非空字符串，浏览器会弹出一个对话框，来询问用户是否确定要离开当前页面（如下示例）。没有赋值时，该事件不做任何响应。

> 出于安全考虑，离开页面的弹框不可定制，只能修改提示内容

示例：
```
window.addEventListener("beforeunload", function (e) {
	var confirmationMessage = "\o/";
	// Gecko and Trident
	(e || window.event).returnValue = confirmationMessage;  
	// Gecko and WebKit   
	return confirmationMessage;                                
});
```
[mdn: beforeunload](https://developer.mozilla.org/zh-CN/docs/Web/Events/beforeunload)

