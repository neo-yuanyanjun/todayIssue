每日问题汇总
==========

*	https 协议下安卓端默认不加载 http协议请求
	* 解决办法 [https://segmentfault.com/q/1010000004727822](https://segmentfault.com/q/1010000004727822)
	* 我的解决办法是（把图片的域名更整体更换成https）
*	zepto touch（最新版） 在chrome 55会tap事件会触发两次[zepto issue](https://github.com/madrobby/zepto/issues/1249)
	* [github zepto issue double tap](https://github.com/madrobby/zepto/issues/1249) 最后选择用 @MarvinXu 的方法移除所有pointer事件，成功解决，tap在chrome上触发两次的问题。
*	zepto 获取input checkbox 是否选中 获取不到！！
	* 有待进一步分析，目前还不确定问题的真正原因...