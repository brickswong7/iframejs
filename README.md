'use strict';
//判断是否是顶层的窗口
/**
 * [iframeObj description]
 * @type {Object}
 * top层找children，传入parent，反之则反
 *  var _iframe = document.getElementById('iframeId').contentWindow;
 *  var _div =_iframe.document.getElementById('objId');
 *  iframeFind函数分析：
 *  type,paramsObj,callbackParams,callback（parent/children,选择器的参数,操作的参数，操作的函数）
 *  isFunction 能够判断Array,String,Data,Function,Boolean,Number等类型
 */
```
function isFunction(fn) {
   return Object.prototype.toString.call(fn)=== '[object Function]';
}
var iframeObj ={
		iframe:"",
		indexIframe:function () {
			if(window.top == window.self){
				this.iframe = "parent";
			}else{
				this.iframe = "children";
			}
		},
		callback:function(){},
		callbackParams:{},
		iframeFind:function(type,paramsObj,callbackParams,callback){
			this.indexIframe();
			//判断参数是否是string
			if(typeof paramsObj != "object")return false;
			var currentLocation =  type == this.iframe ? this.iframe:"error";//这一步完全可以不要。但是为了提醒操作者是怎样通信用的
			switch(currentLocation){
				case 'parent':
					var currentEle = document.querySelector(paramsObj[0]).contentWindow.document.querySelector(paramsObj[1]);
					if( !isFunction(callback) ) return false;
					this.callback=callback;
					this.callbackParams = callbackParams;
					return {
						select:currentEle,
						callback:callback,
						callbackParams:callbackParams
					}
				break;
				case 'children':
					var currentEle = window.parent.document.querySelector(paramsObj[0]);
					if( !isFunction(callback) ) return false;
					this.callback=callback;
					this.callbackParams = callbackParams;
					return {
						select:currentEle,
						callback:callback,
						callbackParams:callbackParams
					}
				break;
				case 'error':
				throw 'iframe层级参数 paramsObj 不对';
	
			}
		}
}	
```

