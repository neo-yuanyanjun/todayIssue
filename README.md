日常问题汇总区
=============

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

* 刚刚收到腾讯新闻的一条推送，86西游记的杨洁导演去世了。怀着悲痛的心情，赶紧去百度百科上检索下杨洁这个人的履历。发现这个页面整体都是灰色的，果断按了下F12（旁边的同事说我：这是病啊，老板）即发现了下面这段代码（是页面整体变灰）：

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

![CSS Filter 兼容性](http://oqpmmru7y.bkt.clouddn.com/todayIssue_cssFilter.png)

*	jquery可以重复绑定相同事件到同一个元素上！！（所以你要确保每个元素身上只绑定了一个事件）这是[stackoverflow 上的解决方案](http://stackoverflow.com/questions/14969960/jquery-click-events-firing-multiple-times)
> 通过这个问题衍生的一些问题：jquery `on`方法绑定事件的原理，是放在一个数组里面的，触发的时候循环数组里面的事件，所以会触发多次。
> 
> 这里还有dom0级事件和dom2级事件（https://github.com/smileyby/dom0_dom2）

这里有必要科普下jquery源码中，on方法的实现原理：

![](http://oqpmmru7y.bkt.clouddn.com/todayIssue_3.png)

![](http://oqpmmru7y.bkt.clouddn.com/todayIssue_2.png)

![](http://oqpmmru7y.bkt.clouddn.com/todayIssue_1.png)

> 上面贴出的部分jquery源码，可以看到on方法中，事件之所以可以一次绑定多个，是因为jquery是用“for”循环绑定你所添加的事件
> 
> 其次，在给同一个元素绑定相同事件不发生覆盖的原因是：jquery调用each方法还是循环去添加事件。
> 
> 其底层的事件添加采用的是dom2级事件，即addEventListener来实现。addEventListener本身就是可以绑定多个相同事件在同一个元素身上。所以就出现的，当你给一个元素重复绑定相同事件不发生覆盖的情况。
> 
> 不了解addEventListener为什么可以绑定多个事件的，可以参考：https://github.com/smileyby/dom0_dom2

* 今天和安卓对接的时候发现，安卓端接收字符串和数字的处理方式不一样，我用js调用安卓方法，传递参数为字符串1，但安卓端接收到的确实0（奇怪啊）。
* transform-origin  的位置会受元素display属性影响 http://codepen.io/airen/pen/Jdvbgr 

* PHPError PHP 5.6 中 Automatically populating $HTTP_RAW_POST_DATA is deprecated and will be removed in a future version
* 解决办法：找到php.ini  文件， 把always_populate_raw_post_data  修改为-1 就行了。`always_populate_raw_post_data=-1` 参考链接：[https://www.bram.us/2014/10/26/php-5-6-automatically-populating-http_raw_post_data-is-deprecated-and-will-be-removed-in-a-future-version/](https://www.bram.us/2014/10/26/php-5-6-automatically-populating-http_raw_post_data-is-deprecated-and-will-be-removed-in-a-future-version/)

* input type=file 其中files属性为只读，不可修改

* 很久没用数组的方法，有点生疏了，在这里记录下`concat`拼接两个数组的时候返回值是拼接后的新数组，`push`向数组最后添加元素，返回值是新数组的长度。（不清楚这两个小东西的返回值，被自己坑了一把）

* 微信小程序中scroll-view标签上貌似不能写循环语句！！

* reg.test is not a function 正则中  比如 var reg = "/^[0-9]$/" 会报  reg.test is not a function 如果 var reg = /^[0-9]$/  就不会有错    因为 这才是正则 正确的表达式

* 命令行合并同目录下所有`.ts`后缀文件
> `copy /b *.ts newTs.ts`

* `addEventListener` 监听事件的类型 `transitionend` `pagehide` `animationend` 都是啥意思？

> 不查不知道，事件监听的 `type`有400多个类型，当时我就震惊了。
> 
> [吓得我赶紧恶补下这些东东--可见监听的事件类型 MDN](https://developer.mozilla.org/zh-CN/docs/Web/Events)
> 
> [关于事件监听函数的参数 MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)

`target.addEventListener(type, listener[, options]);`

options为可选参数对象，可用选项如下：

> capture:Boolean，表示listener会在该类型的事件捕获阶段传播到该EventTarget时触发
> 
> once: Bollean，表示listener在添加之后最多只调动一次。如果是true，listener会在其被调用之后自动移除。
>
> passive:Bollean，表示listener永不会调用preventDefault()。如果listener仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告。
>
> mozSystemGroup:只能在XBL或者是FireFox，chrome使用，这是个Boolean，表示listener被添加到system group
> 更多关于listener的参数请参考上面的链接。


* js代码中使用 `use strict` 啥意思 啊？

> [关于js严格模式详解-阮一峰](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html)

> 后面会专写一篇来研究这个鬼严格模式。

* 事件和函数的结尾为啥可以用逗号（参考陪我分享页）暂时没找到太好的解释[逗号当分号用了？](https://segmentfault.com/q/1010000008082093?_ea=1544899)？

* `linear-gradient` `-webkit-tap-hightlight-color` `outline` 这些属性都是什么作用？

> `linear-gradient`:函数创建一个表示颜色线性渐变的<image>，`background:linear-gradient(135deg, red, blue);`效果还不错哦。
> 
> `-webkit-tap-hightlight-color`:是一个没有标准化的属性，能够设置点击链接的时候出现的高亮颜色。显示给用户的高光是他们成功点击的标识，以及暗示他们点击的元素。这个主要是用在移动端点击去除元素的高亮显示。`-webkit-tap-highlight-color: transparent;` [参考链接](http://ued.ctrip.com/webkitcss/prop/tap-highlight-color.html)
> 
> `outline`：用来设置一个或多个单独的轮廓属性的简写属性例：`:link:hover { outline: 1px solid #000; }`
`:link:hover { outline: solid black 1px; }`[参考链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)

* js页面唤起app，没有则跳转下载（这样做存在问题较大，建议将下载和唤醒app两个功能分开）:

```js

function(){
    location.href = 'zidingyi://zidingyi.com/';
    setTimeout(function(){
	location.href = 'http://xxx.apk';
    }, 800);
}

```

* amazeui中的radio标签内嵌的form表单中是，外层需要嵌套一个类名为 `am-btn-group` 的父元素,这样表单提交的时候才会提交上去。示例如下:

```html

<div class="am-btn-group" data-am-button>
	<label class="am-btn am-btn-default am-btn-sm radiobtn">
	    <input name="selected" value="1" type="radio"/> 选项一
	</label>
</div>

```

* 浏览器请求跨域-关键词`Access-Control-Allow-Origin`
* https 下禁止http请求-关键词`was loaded over HTTPS` `This request has been blocked; the content must be served over HTTPS.`

* 安卓手机QQ浏览器打开页面如果被封，则请求头会带有 403 forbidden 反馈

* `Error saving setting with name: consoleHistory, value length: 5272836. Error: Failed to set the 'consoleHistory' property on 'Storage': Setting the value of 'consoleHistory' exceeded the quota.`[stackoverflow about this console info](https://stackoverflow.com/questions/29277964/copy-paste-big-dictionary-into-chrome-console)

* phpstrom 默认限制打开文件大小为 2.5M，过大则会提示超出内容不予记录。可在配置文件中修改最大容量。（个人感觉这一点上sublime要强很多，同样是打开5M的文件，sublime要流畅很多）

* js获取页面的header头部信息，是否可以？参考网址：https://github.com/smileyby/website-header

* ajax在什么情况下会出现error的情况？
> 出现跨域是会回调error函数、感觉王者这方面的信息偏少。这一块需要自己总结。后面补上吧。

* javascript中的for/try/switch/while/do while/try/hasOwnProperty/闭包。。。

> for：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for

> try：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch

> while:https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/while

> do while：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/do...while

> hasOwnProperty: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty

> 闭包：随机找了两篇文章：http://www.jb51.net/article/24101.htm http://blog.csdn.net/u014205965/article/details/45936997 明天仔细看看
> 我真的是仔细看了，但还是看不懂。该怎么自我拯救一下，我是真的看不懂啊！！！

* javascript中整数相除得到的一定不是整数，为什么？

> 首先说一下这句话是错的，js中整数相除可能会得到一个非整数。还有一点需要注意的就是**js中认为1和1.0是相等的（即：`1 === 1.0`）**,避免了短整数溢出的问题。

* substr用法

```js

/**
    * 获取指定函数的函数名称（用于兼容IE）
    * @param {Function} fun 任意函数
    */
function getFunctionName(fun) {
    if (fun.name !== undefined)
        return fun.name;
    var ret = fun.toString();
    ret = ret.substr('function '.length);
    ret = ret.substr(0, ret.indexOf('('));
    return ret;
}

// 看关于函数的一些东东的时候发现了这个获取函数名的方法，顺带复习下substr用法

```

> String.prototype.substr()
>
> str.substr(start[, length])
>
> substr方法可传入两个参数：
> * start开始提取字符的位置。如果为赋值，则被看做strLength + start，其中strLength为字符串的长度（例如，如果start为-3，则被看做strLength - 3）。
> 
> length可选。提取的字符数
> 
> start 是一个字符的索引。首字符的索引为0，最后一个字符的索引为字符串的长度减1.substr从start位置开始提取字符，提取length个字符（或者到字符串的末尾）。
> 
> 如果start为正值，且大于等于字符串的长度，则substr返回一个空字符串。
> 
> 如果start为负值，则substr把它作为字符串末尾的一个字符索引。如果start为负值且abs(start)大于字符串的长度，则substr使用0作为开始提取的左尹。注意负的start参数不被Microsoft Jscript所支持。
> 
> 如果length为0或者负值，则substr返回一个空字符串。如果忽略length，则substr提取字符串，知道字符串末尾。

```js

// substr 示例代码
var str = "abcdefghij";

console.log("(1,2): "    + str.substr(1,2));   // (1,2): bc
console.log("(-3,2): "   + str.substr(-3,2));  // (-3,2): hi
console.log("(-3): "     + str.substr(-3));    // (-3): hij
console.log("(1): "      + str.substr(1));     // (1): bcdefghij
console.log("(-20, 2): " + str.substr(-20,2)); // (-20, 2): ab
console.log("(20, 2): "  + str.substr(20,2));  // (20, 2):

```

* 关于选择器，jquery中 `:last`和`:last-child`区别？

> `:last`选择该元素同类的最后一个
> 
> `:last-child`选择该元素子元素同类的最后一个
> *一直以为都是选择元素的最后一个元素，结果就被坑了一波。。。。*

* this指向问题，在字面量函数中，this为什么会指向window？

* Object.prototype.propertyIsEnumerable()

> propertyIsEnumerable() 方法返回一个布尔值，表明指定的属性名是否是当前对象可枚举的自身属性。

* form表单提交 **button的id不要设置为submit,否则可能会引起混淆，导致表单的submit()方法不能提交表单。在命名ID时，名字最好不要和现有的api在名称上重复，避免不必要的烦扰**

* 经常在git上看到五颜六色的字体，不知道人家是怎么实现的，刚所搜了一下原来是用<font></font>标签包裹并给出字体颜色和大小即可。

```html

<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=7 face="黑体">color=#0099ff size=72 face="黑体"</font>
<font color=#00ffff size=72>color=#00ffff</font>
<font color=gray size=72>color=gray</font>

```

* input file 标签 value 不能赋值
> 出于安全考虑，input的type=file value值是不能动态修改的

* 最近在爬数据想用node-crawler来爬，结果发现node.js版本过低不能使用，检索了一下，发现可以使用如下更新方法：
> 1、首先安装n模块

> 别看它名字很短，用途却很大，可以用短小精悍来形容。n模块是专门用来管理nodejs版本的。

> 这里需要全局安装，命令如下：

> npm install -g n

> 2、升级node.js到最新稳定版

> 命令如下：

> n stable

结果，报错显示如下图：

![](http://oqpmmru7y.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170801163334.png)

> 在检索一下，原来是n模块不支持window系统，无奈只能去安装程序中吧nodejs卸载了，下载一个最新版本的重新安装。
> 参考：[https://www.npmjs.com/package/n](https://www.npmjs.com/package/n)

* 不同版本的app绝壁不要用同一个付费页，各种坑，一个版本新建一个文件夹确保不影响其他版本，地址后面拼接参数的方式真的是恶心到爆炸。

* background-attachment 控制背景图

> 该属性可用于实现页面的背景固定特效 参见: https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment

* 使用360断网急救箱，强制重置网络配置。它把我HOSTS文件里面的全部配置都注释了，导致开启phpstuday不能访问本地环境。

* https://segmentfault.com/a/1190000000385867 node request模块使用，主要看看咋设置headers

* TDD---TDD是测试驱动开发（Test-Driven Development）的英文简称，是敏捷开发中的一项核心实践和技术，也是一种设计方法论。TDD的原理是在开发功能代码之前，先编写单元测试用例代码，测试代码确定需要编写什么产品代码。TDD虽是敏捷方法的核心实践，但不只适用于XP（Extreme Programming），同样可以适用于其他开发方法和过程。

* 为什么会报错？

![](http://oqpmmru7y.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170816184026.png)

已解决因为在引用时还未定义

* input 添加 `required` 属性表示必须填写才能提交，在safari下不能使用

* jquery中的appendTo方法可以使选中元素移动到指定的元素中  $("#target4").appendTo("#left-well"); 

* 一个比较好玩的加载效果 --- http://demo.geekslabs.com/materialize-v1.0/index.html 

* ios webview input获取焦点时会自动放大页面，解决添加禁止缩放的meta标签

* UTC和GMT时间标准的区别 及转化 http://www.jb51.net/article/77636.htm

* 为什么undefined、NaN和Infinity可以被赋值，而null不可以？ https://segmentfault.com/q/1010000004181674

* form表单提交数据，如果select设置了disable属性，则提交时select数据不会被提交

* VIP卡片 阴影特效 `box-shadow: 0 10px 20px 0 rgba(0,0,0,0.18);`

* iframe内容在ios下会被放大，怎么解决呢？ （参考链接：http://www.cnblogs.com/zichi/p/5078115.html，http://www.cnblogs.com/yanqin/p/7156022.html）

* `Node.appendChild()`方法将一个节点添加到指定父节点的子节点列表末尾。
> 注意appendChild(child) `child`参数是节点，例如`var child = document.createElement("p");`，而不是字符串。
>
> 如果使用appendChild()方法参数为字符串将报错如下：
> 
> `Uncaught TypeError: Failed to execute 'appendChild' on 'Node': parameter 1 is not of type 'Node'.`

* jquery 和 zepto 中使用 `$`符实例化dom对象，应多赋值给变量避免不断实例化dom对象耗费内存资源。

* ios端使用 `use-select:none;` input不能获取焦点问题 参考链接：http://blog.csdn.net/songchunmin_/article/details/52846719

* 模拟按钮点击波纹效果，参考链接：http://down.admin5.com/demo/code_pop/19/1662/

* 寻找适合移动端 上传图片并压缩的 js控件，参考链接：[http://imweb.io/topic/568225bb57d7a6c47914fbf2](http://imweb.io/topic/568225bb57d7a6c47914fbf2)，[https://github.com/fex-team/webuploader/](https://github.com/fex-team/webuploader/)，[https://github.com/xfhxbb/LUploader](https://github.com/xfhxbb/LUploader)

> 遇到很多坑啊，在ios端本来是想做一步压缩操作，用canvas来压缩，在正式环境测了一波发现，压缩失败一番检索发现ios中使用canvas压缩图片超过2M自动中断，参考链接： https://www.zhihu.com/question/30692677
>
> 后面有时间要专门写一篇移动端上传图片的文档，让后面的人知道坑在哪里。详情查看：https://github.com/smileyby/js-upload-img

* js 时间戳转北京时间，详情查看：https://github.com/smileyby/times-stamp

* 发现项目的古老三星测试机变砖了，不能忍马上百度了一波解决方案。（错误提示：`SecSetupWizard已停止`），解决办法：http://tieba.baidu.com/p/2933537636

* 有时间要弄一个下拉刷新上拉加载的插件，总是用别人家的感觉很不舒服。有一些需求满足不了。哎

* 将weui中的部分样式独立出来，不需要引入整个weui库文件（https://smileyby.github.io/smallPart)

* form表单向后台提交数组格式数据 `<input type="text" name="thisObj[]" value="">`

* margin padding translate 等属性的 50% 都是相对于谁的？

```js

for (var i = 1; i <= 5; i += 1){
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 )
}

```

* 这段代码为什么会输出5个6？？？？？

* try catch 用法是怎样的？
