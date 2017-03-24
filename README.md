## 每日问题汇总

*	https 协议下安卓端默认不加载 http协议请求
	* 解决办法 [https://segmentfault.com/q/1010000004727822](https://segmentfault.com/q/1010000004727822)
	* 我的解决办法是（把图片的域名更整体更换成https）
*	zepto touch（最新版） 在chrome 55会tap事件会触发两次[zepto issue](https://github.com/madrobby/zepto/issues/1249)
	* [github zepto issue double tap](https://github.com/madrobby/zepto/issues/1249) 最后选择用 @MarvinXu 的方法移除所有pointer事件，成功解决，tap在chrome上触发两次的问题。
*	zepto 获取input checkbox 是否选中 获取不到
	*	需要引入zepto的`selector.js`模块
*	【错误处理】:Uncaught SyntaxError: Invalid or unexpected token
	* 这个错误是因为，js代码中存在非英文标点符号（仔细检查下代码）
*	[css中有些属性的前面会加上`*`或`_`分别代表什么意思？](http://blog.csdn.net/yohoph/article/details/7759372)
	* 这些是IE6，IE7，IE8，Firefox兼容的css hack	
	* 很多css hack，最重要的是简单实用能解决问题就行了。
	* `\9`：IE6/IE7/IE8
	* `*`：IE6/IE7
	* `_`：IE6
	* `*+`：IE7
	* 先看一个例子：

```css

color {
	background-color: #CC00FF; /* 所有浏览器都会显示为紫色 */
	background-color: #FF0000\9; /* IE6、IE7、IE8会显示红色 */
	*background-color: #0066FF;  /* IE6、IE7会变为蓝色 */   
	_background-color: #009933;  /* IE6会变为绿色 */
}

```
	

## 每日有趣网址收集

http://k.swao.cn/js/web/game/t_01/level_01/step1.html	