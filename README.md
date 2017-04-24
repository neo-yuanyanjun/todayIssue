## 问题汇总和日常总结

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

*	image跨域访问问题
	* canvas 调用drawImage方法会出现	
	* 我的解决办法是把图片转成base64（缺点，如果图片很大会增加页面代码量）
	* [segmentfault 上的解决办法](https://segmentfault.com/q/1010000000768672)
	* [MDN 解决方案](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_enabled_image)

```
Image from origin 'http://sdimage.b0.upaiyun.com' has been blocked from loading by Cross-Origin Resource Sharing policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:3000' is therefore not allowed access.

```

*	`document.referrer` (**同一个域名下**获取本页面上一个页面的地址信息，若没有则返回空字符串)
*	使用Js去除“内容”的前后空格

```js

function Trim(str)
{ 
 	return str.replace(/(^\s*)|(\s*$)/g, ""); 
}

// 如果使用jQuery直接使用$.trim(str)方法即可，str表示要去掉前后所有空格的字符串

```

*	去掉字符串中所有空格（包括中间空格）

```js

 function Trim(str,is_global)
{
    var result;
    result = str.replace(/(^\s+)|(\s+$)/g,"");
    if(is_global.toLowerCase()=="g")
    {
        result = result.replace(/\s/g,"");
     }
    return result;
}

```

*	刚刚收到腾讯新闻的一条推送，86西游记的杨洁导演去世了。怀着悲痛的心情，赶紧去百度百科上检索下杨洁这个人的履历。发现这个页面整体都是灰色的，果断按了下F12（旁边的同事说我：这是病啊，老板）即发现了下面这段代码（是页面整体变灰）：

```css

/* 采用css3的灰度过滤，是页面整体变灰 */
body.memorial {
    filter: grayscale(100%);
    -webkit-filter: grayscale(100%);
    -moz-filter: grayscale(100%);
    -ms-filter: grayscale(100%);
    -o-filter: grayscale(100%);
    filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);
    -webkit-filter: grayscale(1);
}

```

**兼容性：**[CSS Filter Effects](http://www.caniuse.com/#search=filter)

![CSS Filter 兼容性](images/cssFilter.png)

*	jquery可以重复绑定相同事件到同一个元素上！！（所以你要确保每个元素身上只绑定了一个事件）这是[stackoverflow 上的解决方案](http://stackoverflow.com/questions/14969960/jquery-click-events-firing-multiple-times)

	

## 有趣网址收集

http://k.swao.cn/js/web/game/t_01/level_01/step1.html	
