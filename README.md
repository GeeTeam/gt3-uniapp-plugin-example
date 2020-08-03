# DCloud 插件集成文档

# 引用方式

```js
var gtSDKModule = uni.requireNativePlugin("Geetest-GT3CaptchaModule");
```

# Android

## 前置条件

- 极验 SDK 支持 Android Studio 2.1.3 及以上版本，Android 5.0及以上版本
- 极验 SDK uni-app 插件支持 HBuildX 2.7.5 及以上版本

## 行为验证 API 使用说明

插件提供两种方式实现行为验证功能：

1. 默认方式：传入 api1、api2，插件根据 api1、api2 进行请求获取参数，完成行为验证功能（**Android 此方式不支持修改上下行参数，若需要修改请使用方式 2**）
2. 自定义方式（即异步任务方式）：完全自定义 api1、api2，自行进行 api1、api2 的请求，将 api1、api2 的结果传给插件，完成行为验证功能

### 初始化

**方法描述**

SDK 初始化。

**代码示例**

```js
gtSDKModule.initWithAPI1("http://www.geetest.com/demo/gt/register-test", "http://www.geetest.com/demo/gt/validate-test");
```

- 接口参数说明：

| 参数 | 类型   | 说明            |
| ---- | ------ | --------------- |
| api1 | string | api1 对应的 URL |
| api2 | string | api2 对应的 URL |

### 设置行为验证使用的语言

**方法描述**

设置行为验证使用的语言，包括 loading 状态、弹窗、验证结果指示等。

**代码示例**

```js
gtSDKModule.setLang("zh");
```

- 接口参数说明：

| 参数 | 类型   | 说明     |
| ---- | ------ | -------- |
| type | String | 语言类型 |

支持的语言及语言短码如下：

```
/**
 *  "zh" - 简体中文
 *  "zh-tw" - 繁体中文
 *  "zh-hk" - 繁体中文
 *  "ko" - 韩语
 *  "ja" - 日语
 *  "en" - 英语
 *  "id" - 印度尼西亚语
 *  "ar" - 阿拉伯语
 *  "de" - 德语
 *  "es" - 西班牙语
 *  "fr" - 法语
 *  "pt-pt" - 葡萄牙语
 *  "ru" - 俄语
 */
```

### 设置是否禁用弹窗蒙板交互

**方法描述**

为 true时，点击弹窗蒙板可关闭弹窗，为 false时，点击弹窗蒙板不能关闭弹窗。

**代码示例**

```js
gtSDKModule.setCanceledOnTouchOutside(false);
```

- 接口参数说明：

| 参数    | 类型    | 说明                 |
| ------- | ------- | -------------------- |
| disable | boolean | 是否禁用弹窗蒙板交互 |

### 设置服务器节点

**方法描述**

设置行为验证使用的服务器节点。

**代码示例**

```js
gtSDKModule.setGt3ServiceNode(0);
```

- 接口参数说明：

| 参数 | 类型   | 说明     |
| ---- | ------ | -------- |
| type | number | 节点类型 |

行为验证支持的节点如下：

```
/**
 * 0 - 中国服务集群
 * 1 - 北美服务集群，北美集群请联系极验完成相关配置方可使用
 */
```

### 设置 webview 答案校验接口超时时间

**方法描述**

设置 webview 答案校验接口超时时间，默认10000毫秒

**代码示例**

```js
gtSDKModule.setWebviewTimeout(10000);
```

- 接口参数说明：

| 参数   | 类型 | 说明                       |
| ------ | ---- | -------------------------- |
| number | int  | 请求接口超时时长，单位毫秒 |

### 设置 webview 资源加载超时

**方法描述**

设置 webview 资源加载超时，默认10000毫秒

**代码示例**

```js
gtSDKModule.setTimeout(10000);
```

- 接口参数说明：

| 参数   | 类型 | 说明                           |
| ------ | ---- | ------------------------------ |
| number | int  | 静态资源加载超时时长，单位毫秒 |

### 获取行为验证 SDK 的版本号

**方法描述**

获取 Android SDK 的版本号。

**代码示例**

```js
let sdkVersion = gtSDKModule.sdkVersion();
console.log("=========== sdkVersion: =========== " + sdkVersion);
```

- 接口返回参数说明：

| 参数       | 类型   | 说明                 |
| ---------- | ------ | -------------------- |
| sdkVersion | string | Android SDK 的版本号 |

### 开启验证 (默认方式)

**方法描述**

开启验证，弹出行为验证弹窗，接收验证结果。

**代码示例**

```js
// android 推荐使用异步集成，默认集成无法对api1、api2上行参数下行内容做修改
// 开启验证
gtSDKModule.startCaptcha((result) => {
	// result为验证成功与否结果回调
	console.log("=========== captcha result ======= " + JSON.stringify(result));
	if (typeof result.error_code == "undefined" || result.error_code == null || result.error_code == "") {
		uni.showToast({
			icon: "none",
			title: "验证成功",
			duration: 2000
		});
	} else {
		uni.showToast({
			icon: "none",
			title: "验证失败:" + result.error_code,
			duration: 2000
		});
	}
}, (api2CallBack) => {
	// api2CallBack为二次验证请求参数回调
	console.log("=========== captcha js result ======= " + JSON.stringify(api2CallBack));
	if (api2CallBack != null) {
		let geetest_challenge = api2CallBack.geetest_challenge;
		let geetest_seccode = api2CallBack.geetest_seccode;
		let geetest_validate = api2CallBack.geetest_validate;
		console.log("=========== geetest_challenge: " + geetest_challenge + ", geetest_seccode: " + geetest_seccode + ", geetest_validate: " + geetest_validate + " =======");
	}
}, (closedResult) => {
	// closedResult为关闭弹框方式回调
	// 1 点击验证码的关闭按钮来关闭验证码, 2 点击屏幕关闭验证码, 3 点击返回键关闭验证码
	console.log("=========== captcha js result ======= " + closedResult.closedNumber);
});
```

- 接口参数说明：

| 参数         | 类型     | 说明                                     |
| ------------ | -------- | ---------------------------------------- |
| result       | function | 验证结果回调                             |
| api2CallBack | function | 二次验证请求参数回调，此方式下可不用关注 |
| closedResult | function | 行为验证弹窗关闭方式                     |

- result 返回参数描述：

成功则无具体返回值，失败则返回错误码及错误描述

| 字段       | 类型   | 含义     |
| ---------- | ------ | -------- |
| error_code | string | 错误码   |
| error_msg  | string | 返回信息 |

- api2CallBack 返回参数描述：

成功时返回参数如下（默认方式下无需关注此回调）：

```json
{"geetest_seccode":"b62eb8e1569dada872242124cf1b9be5|jordan","geetest_validate":"b62eb8e1569dada872242124cf1b9be5","geetest_challenge":"a24cb2a5bc7b03854a04bf53f67cb136k9"}
```

* closedResult 返回参数描述

关闭弹框方式回调，返回参数描述如下：

| 参数 | 类型     | 说明                             |
| ---- | -------- | -------------------------------- |
| 1    | int      | 点击验证码的关闭按钮来关闭验证码 |
| 2    | function | 点击屏幕关闭验证码               |
| 3    | function | 点击返回键关闭验证码             |

### 开启验证 (自定义方式)

**方法描述**

开启验证，弹出行为验证弹窗，接收验证结果。

**代码示例**

```js
// 开启验证			
gtSDKModule.startAsyncTaskCaptcha((result) => {
	// result为验证成功与否结果回调
	console.log("=========== captcha result ======= " + JSON.stringify(result));
	if (typeof result.error_code == "undefined" || result.error_code == null || result.error_code == "") {
		uni.showToast({
			icon: "none",
			title: "验证成功",
			duration: 2000
		});
	} else {
		uni.showToast({
			icon: "none",
			title: "验证失败:" + result.error_code,
			duration: 2000
		});
	}
}, (api1CallBack) => {
	// api1CallBack为 api1 请求开启回调
	this.$options.methods.startRegisterTask();
	
}, (api2CallBack) => {
	// api2CallBack为二次验证请求参数回调，并在此开启二次验证
	console.log("=========== captcha js result ======= " + JSON.stringify(api2CallBack));
	this.$options.methods.startValidateTask(JSON.stringify(api2CallBack));
	
}, (closedResult) => {
	// closedResult为关闭弹框方式回调
	// 1 点击验证码的关闭按钮来关闭验证码, 2 点击屏幕关闭验证码, 3 点击返回键关闭验证码
	console.log("=========== captcha js result ======= " + closedResult.closedNumber);
	
});
```

- 接口参数说明：

| 参数         | 类型     | 说明                                           |
| ------------ | -------- | ---------------------------------------------- |
| result       | function | 验证结果回调                                   |
| api1CallBack | function | 于此回调发起 api1 请求                         |
| api2CallBack | function | 二次验证请求参数回调，并于此回调发起 api2 请求 |
| closedResult | function | 行为验证弹窗关闭方式                           |

- result 返回参数描述：

成功则无具体返回值，失败则返回错误码及错误描述

| 字段       | 类型   | 含义     |
| ---------- | ------ | -------- |
| error_code | string | 错误码   |
| error_msg  | string | 错误描述 |

- api1CallBack 返回参数描述：

无，此回调用于通知 js 发起 api1 请求

- api2CallBack 返回参数描述：

图形验证成功后返回参数如下（此三个参数用于发起 api2 请求）：

```json
{"geetest_seccode":"b62eb8e1569dada872242124cf1b9be5|jordan","geetest_validate":"b62eb8e1569dada872242124cf1b9be5","geetest_challenge":"a24cb2a5bc7b03854a04bf53f67cb136k9"}
```

* closedResult 返回参数描述

关闭弹框方式回调，返回参数描述如下：

| 参数 | 类型     | 说明                             |
| ---- | -------- | -------------------------------- |
| 1    | int      | 点击验证码的关闭按钮来关闭验证码 |
| 2    | function | 点击屏幕关闭验证码               |
| 3    | function | 点击返回键关闭验证码             |

api1、api2 请求示例如下：

```js
startRegisterTask() {
	// 通过 api1 请求拉取 gt、challenge、success 参数
	let timestamp = new Date().getTime();
	let url = "http://www.geetest.com/demo/gt/register-test?t=" + timestamp;
	uni.request({
		url: url,
		success: (res) => {
			console.log("=========== async task api1 result ======= ");
		    console.log(res.data);
			let gt = res.data.gt;
			let challenge = res.data.challenge;
			let success = res.data.success;
			if (null != gt && null != challenge && null != success) {
                // 传入 api1 请求结果，请严格按照此格式传入
				gtSDKModule.setAsyncTaskRegisterResult({
					"gt": gt,
					"challenge": challenge,
					"success": success
				});
			} else {
				gtSDKModule.setAsyncTaskRegisterResult({
					"error": "invalidate response from api1"
				});
			}
		}, 
		fail: (error) => {
			console.log("=========== async task api1 error ======= ");
			console.log(error);
			gtSDKModule.setAsyncTaskRegisterResult({
				"error": error.description
			});
		}
	});
},
startValidateTask(result) {
	// 行为验证弹窗上手动操作验证通过之后，进行二次验证
	console.log("=========== start async validate task ======= ");
	if (typeof result === 'string') {
		console.log("=========== async register task result: " + result + " ======= ");
		let json = JSON.parse(result);
		console.log("=========== json's type: ======= " + typeof json);
		var postData = "";
		for (var key in json) {
			if (key == "result") {
				let jsonResult = json.result;
				console.log("=========== jsonResult: " + JSON.stringify(jsonResult) + " =======");
				for (var jsonKey in jsonResult) {
					postData += (jsonKey + "=" + jsonResult[jsonKey] + "&");
				}
			} else {
				postData += (key + "=" + json[key] + "&");
			}
		}
		if (postData.length > 1) {
			postData = postData.substring(0, postData.length - 1);
		}
		console.log("=========== postData: " + postData + " =======");
		uni.request({
			url: "http://www.geetest.com/demo/gt/validate-test",
			data: postData,
			header: {"Content-Type": "application/x-www-form-urlencoded;charset=UTF-8"},
			method: "POST",
			success: (res) => {
				console.log("=========== async task api2 result ======= ");
				console.log("=========== " + res.data + " =======");
				let status = res.data.status;
				let info = res.data.info;
				console.log("=========== status: " + status + ", info: " + info + " =======");
				// 判断是否验证成功
				if (status == "success") {
					// 将二次验证结果回传给插件
					gtSDKModule.setAsyncTaskValidateResult({
						"result": "true"
					});
				} else {
					// 将二次验证结果回传给插件
					gtSDKModule.setAsyncTaskValidateResult({
						"result": "fasle",
						"error": info
					});
				}
			},
			fail: (error) => {
				console.log("=========== async task api2 error ======= ");
				console.log(error);
				// 将二次验证结果回传给插件
				gtSDKModule.setAsyncTaskValidateResult({
					"result": "false",
					"error": error.description
				});
			}
		});
	} else {
		gtSDKModule.setAsyncTaskValidateResult({
			"result": "false",
			"error": "api2 validate failed"
		});
	}
}
```

### 回传 api1 请求结果给插件

setAsyncTaskRegisterResult (JSON json)

**方法描述**

**自定义方式集成**时，api1 请求完成之后，将结果回传给插件。

**代码示例**

```js
let gt = res.data.gt;
let challenge = res.data.challenge;
let success = res.data.success;
if (null != gt && null != challenge && null != success) {
	gtSDKModule.setAsyncTaskRegisterResult({
        // 传入 api1 请求结果，请严格按照此格式传入
		"gt": gt,
		"challenge": challenge,
		"success": success
	});
} else {
	gtSDKModule.setAsyncTaskRegisterResult({
		"error": "invalidate response from api1"
	});
}
```

- 接口参数说明：

| 参数   | 类型 | 说明              |
| ------ | ---- | ----------------- |
| result | json | api1 接口返回参数 |

### 回传 api2 请求结果给插件

setAsyncTaskValidateResult (JSON json)

**方法描述**

**自定义方式集成**时，api2 请求完成之后，将结果回传给插件。

**代码示例**

```js
let status = res.data.status;
let info = res.data.info;
console.log("=========== status: " + status + ", info: " + info + " =======");
// 判断是否验证成功
if (status == "success") {
	// 将二次验证结果回传给插件
	gtSDKModule.setAsyncTaskValidateResult({
		"result": "true"
	});
} else {
	// 将二次验证结果回传给插件
	gtSDKModule.setAsyncTaskValidateResult({
		"result": "fasle",
		"error": info
	});
}
```

- 接口参数说明：

| 参数   | 类型 | 说明          |
| ------ | ---- | ------------- |
| result | json | api2 校验结果 |

校验成功时，参数格式为：

```json
{"result": "true"}
```

校验失败时，参数格式为：

```json
{"result": "false", "error": error.description}
```

# iOS

## 前置条件

- 极验 SDK 支持 Xcode 11+，iOS 8.0+ 版本
- 极验 SDK uni-app 插件支持 HBuildX 2.6.16 及以上版本

## 行为验证 API 使用说明

插件提供两种方式实现行为验证功能：

1. 传入 api1、api2，插件根据 api1、api2 进行请求获取参数，完成行为验证功能
2. 完全自定义 api1、api2，自行进行 api1、api2 的请求，将 api1、api2 的结果传给插件，完成行为验证功能

### 初始化

**方法描述**

SDK 初始化。

**代码示例**

```js
gtSDKModule.initWithAPI1("http://www.geetest.com/demo/gt/register-test", "http://www.geetest.com/demo/gt/validate-test", 5.0);
```

- 接口参数说明：

参数|类型|说明
---|---|---
api1|string|api1 对应的 URL
api2|string|api2 对应的 URL
timeout|number|超时时间，单位:`s`，默认`5`

### 注册验证

**方法描述**

注册验证，提前进行 api1 请求。

**代码示例**

```js
gtSDKModule.registerCaptchaWithCallback((result)=> {
	console.log("=========== register captcha =========== ");
});
```

- 接口参数说明：

参数|类型|说明
---|---|---
callback|function|注册完成的回调

### 开启验证

**方法描述**

开启验证，弹出行为验证弹窗，接收验证结果。

**代码示例**

```js
// 开启验证
gtSDKModule.startGTCaptchaWithAnimated(true, (result) => {
	console.log("=========== captcha result ======= " + JSON.stringify(result));
	let status = result.status;
	let info = result.info;
	// 判断是否验证成功
	if (status == "success") {
		uni.showToast({
			icon: "none",
			title: "验证成功",
			duration: 2000
		});
	} else {
		uni.showToast({
			icon: "none",
			title: "验证失败:" + info,
			duration: 2000
		});
	}
}, (jsResult) => {
	console.log("=========== captcha js result ======= " + JSON.stringify(jsResult));
	let code = jsResult.code;
	let result = jsResult.result;
	if (result != null) {
		let geetest_challenge = result.geetest_challenge;
		let geetest_seccode = result.geetest_seccode;
		let geetest_validate = result.geetest_validate;
		console.log("=========== code: " + code + ", geetest_challenge: " + geetest_challenge + ", geetest_seccode: " + geetest_seccode + ", geetest_validate: " + geetest_validate + " =======");
	}
});
```

- 接口参数说明：

参数|类型|说明
---|---|---
animated|boolean|弹窗出现时是否需要动画
gt3Callback|function|验证结果回调
jsHandlerCallback|function|行为验证弹窗上处理滑动、点击等动作的 js 结果回调

- gt3Callback 返回参数描述：

返回参数根据您自己的接口返回参数确定，以下展示的是极验 api2 验证返回的参数

```json
{"status":"success","info":"登录成功"}
```

字段|类型|含义
---|---|---
status|string|验证状态
info|string|返回信息

- jsHandlerCallback 返回参数描述：

成功时返回参数如下：

```json
{"result":{"geetest_challenge":"b3fa94de2f0431dcd840787c6e326aadb6","geetest_seccode":"d4937dffb1e93a2c60e8dec9a08541e6|jordan","geetest_validate":"d4937dffb1e93a2c60e8dec9a08541e6"},"code":"1"}
```

失败时返回参数如下：

```json
{"result":{"geetest_challenge":""},"code":"0"}
```

字段|类型|含义
---|---|---
result|json|`{"geetest_challenge":"b3fa94de2f0431dcd840787c6e326aadb6","geetest_seccode":"d4937dffb1e93a2c60e8dec9a08541e6|jordan","geetest_validate":"d4937dffb1e93a2c60e8dec9a08541e6"}`
code|string|1: 成功 0: 失败

### 修改 api1 request

**方法描述**

插件会以固定的方式去进行 api1 请求，如果 api1 请求有特殊的需求，比如需要定制 http header，可以修改 api1 的 request。

**代码示例**

```js
/** 若要修改 api1 的请求或者响应，需要在 registerCaptchaWithCallback 方法前调用 */

// 若要修改 api1 request，需调用如下两个接口
gtSDKModule.setModifyAPI1Request(true);
gtSDKModule.customAPI1Request({
	"url": "",
	"cachePolicy": 4,
	"timeInterval": 5.0,
	"HTTPMethod": "GET",
	"HTTPHeaderFields": {
		
	},
	"HTTPBody": {
		
	}
});
```

- setModifyAPI1Request 接口参数说明：

参数|类型|说明
---|---|---
modifyAPI1Request|boolean|是否修改 api1 request

- customAPI1Request 接口参数说明：

参数|类型|说明
---|---|---
api1Request|json|api1 request

- api1Request 字段说明：

参数|类型|说明
---|---|---
url|string|api1 请求 url
cachePolicy|number|缓存策略
timeInterval|number|超时时长
HTTPMethod|string|请求方式
HTTPHeaderFields|json|http header
HTTPBody|json|http body


<font color="#ff0000">注意：若要修改 api1 的请求或者响应，需要在 registerCaptchaWithCallback 方法前调用</font>

### 修改 api1 response

**方法描述**

插件会以固定的方式去进行 api1 请求，如果 api1 请求有特殊的需求，比如需要在 api1 请求结果中添加某个字段，可以修改 api1 的 response，再将修改之后的 response 回传给插件。

**代码示例**

```js
/** 若要修改 api1 的请求或者响应，需要在 registerCaptchaWithCallback 方法前调用 */

// 修改 api1 的响应，配合 customAPI1Response 方法完成修改 api1 响应的功能
gtSDKModule.setModifyAPI1Response(true);

customAPI1Response(apiResponse) {	// 修改 api1 的响应
	console.log("=========== customAPI1Response ======= ");
	console.log("=========== apiResponse's type: ======= " + typeof apiResponse);
	if (typeof apiResponse === 'string') {
		console.log("=========== apiResponse: ======= " + apiResponse);
		let apiResponseJson = JSON.parse(apiResponse);
		console.log("=========== apiResponseJson's type: ======= " + typeof apiResponseJson);
		
		let gt = apiResponseJson.gt;
		let challenge = apiResponseJson.challenge;
		let success = apiResponseJson.success;
		console.log("=========== apiResponse, gt: " + gt + "challenge: " + challenge + "success: " + success + " =======");
		
		// 自定义 api1 的 response
		if (gt !== null && challenge !== null && success !== null) {
			let json = {
				"gt": gt,
				"challenge": challenge,
				"success": success
			};
			api1Response = JSON.stringify(json);
			// 一定要调用该方法将修改后的 response 回传给插件
			gtSDKModule.customAPI1Response(json);
		}
	}
}
```

- setModifyAPI1Response 接口参数说明：

参数|类型|说明
---|---|---
modifyAPI1Response|boolean|是否修改 api1 response

- setModifyAPI1Response 接口参数说明：

参数|类型|说明
---|---|---
customAPI1Response|json|插件请求 api1 接口返回的数据

customAPI1Response 数据格式如下：

```js
{"success":1,"new_captcha":true,"gt":"4faf9dd37eb683586977947ce87a866a","challenge":"662f64ca8db8cec724c5ef2e09969c73"}
```

<font color="#ff0000">注意：<br>
1、若要修改 api1 的请求或者响应，需要在 registerCaptchaWithCallback 方法前调用；<br>
2、apiResponse 为插件请求 api1 接口返回的数据，您可以根据 apiResponse，获取您需要的数据，再将其它 apiResponse 中没有的数据拼接完成之后，再将 response 回传给插件
</font>

### 修改 api2 request

**方法描述**

插件会以固定的方式去进行 api2 请求，如果 api2 请求有特殊的需求，比如需要定制 http header，可以修改 api2 的 request。api2 请求，需要在 startGTCaptchaWithAnimated 方法的 jsResult 回调中调用。

**代码示例**

```js
let code = jsResult.code;
let result = jsResult.result;
if (result != null) {
	let geetest_challenge = result.geetest_challenge;
	let geetest_seccode = result.geetest_seccode;
	let geetest_validate = result.geetest_validate;
	console.log("=========== code: " + code + ", geetest_challenge: " + geetest_challenge + ", geetest_seccode: " + geetest_seccode + ", geetest_validate: " + geetest_validate + " =======");
	
	if ("1" == code) {
		console.log("=========== custom api2 request =======");
		var postData = "code=" + code + "&";
		for (var key in result) {
			postData += (key + "=" + result[key] + "&");
		}
		if (postData.length > 1) {
			postData = postData.substring(0, postData.length - 1);
		}
		console.log("=========== postData: " + postData + " =======");
		
		// 修改 api2 的请求
		gtSDKModule.setModifyAPI2Request(true);
		gtSDKModule.customAPI2Request({
			"url": "",
			"cachePolicy": 4,
			"timeInterval": 5.0,
			"HTTPMethod": "POST",
			"HTTPHeaderFields": {
				"Content-Type": "application/x-www-form-urlencoded;charset=UTF-8"
			},
			"HTTPBody": postData
		});
	}
}
```

- setModifyAPI2Request 接口参数说明：

参数|类型|说明
---|---|---
modifyAPI2Request|boolean|是否修改 api2 request

- customAPI2Request 接口参数说明：

参数|类型|说明
---|---|---
api2Request|json|api2 request

- api2Request 字段说明：

参数|类型|说明
---|---|---
url|string|api2 请求 url
cachePolicy|number|缓存策略
timeInterval|number|超时时长
HTTPMethod|string|请求方式
HTTPHeaderFields|json|http header
HTTPBody|json|http body

### 设置日志开关

**方法描述**

设置 GT3Captcha.framework 的日志开关，打开开关后，调用 SDK 完成行为验证过程中，会打印相关日志。

**代码示例**

```js
gtSDKModule.setLogEnabled(true);
```

- 接口参数说明：

参数|类型|说明
---|---|---
enabled|boolean|是否打开日志开关

### 获取行为验证 SDK 的版本号

**方法描述**

获取 GT3Captcha.framework 的版本号。

**代码示例**

```js
let sdkVersion = gtSDKModule.sdkVersion();
console.log("=========== sdkVersion: =========== " + sdkVersion);
```

- 接口返回参数说明：

参数|类型|说明
---|---|---
sdkVersion|string|GT3Captcha.framework 的版本号

### 设置行为验证使用的语言

**方法描述**

设置行为验证使用的语言，包括 loading 状态、弹窗、验证结果指示等。

**代码示例**

```js
gtSDKModule.useLanguage(5);
```

- 接口参数说明：

参数|类型|说明
---|---|---
type|number|语言类型

行为验证支持的语言如下：

```
/**
 *  0 - 简体中文
 *  1 - 繁体中文
 *  2 - 繁体中文
 *  3 - 韩语
 *  4 - 日语
 *  5 - 英语
 *  6 - 印度尼西亚语
 *  7 - 阿拉伯语
 *  8 - 德语
 *  9 - 西班牙语
 *  10 - 法语
 *  11 - 葡萄牙语
 *  12 - 俄语
 *  999 - 跟随系统语言
 */
```

### 设置服务器节点

**方法描述**

设置行为验证使用的服务器节点。

**代码示例**

```js
gtSDKModule.useServiceNode(0);
```

- 接口参数说明：

参数|类型|说明
---|---|---
type|number|节点类型

行为验证支持的节点如下：

```
/**
 * 0 - 中国服务集群
 * 1 - 北美服务集群，北美集群请联系极验完成相关配置方可使用
 */
```

### 设置弹窗圆角

**方法描述**

设置行为验证弹窗的圆角大小。

**代码示例**

```js
gtSDKModule.useGTViewWithCornerRadius(5.0);
```

- 接口参数说明：

参数|类型|说明
---|---|---
cornerRadius|number|圆角大小

### 设置是否禁用弹窗蒙板交互

**方法描述**

为 false 时，点击弹窗蒙板可关闭弹窗，为 true 时，点击弹窗蒙板不能关闭弹窗。

**代码示例**

```js
gtSDKModule.disableBackgroundUserInteraction(false);
```

- 接口参数说明：

参数|类型|说明
---|---|---
disable|boolean|是否禁用弹窗蒙板交互

### 处理验证弹窗关闭情况

**方法描述**

处理验证弹窗关闭情况。

**代码示例**

```js
// 处理验证弹窗关闭情况
gtSDKModule.handleGTViewClose((result) => {
	console.log("=========== gt view is closed ======= ");
});
```

- 接口参数说明：

参数|类型|说明
---|---|---
closeGTViewCallback|function|弹窗关闭回调

### 异步任务方式集成初始化

**方法描述**

异步任务方式集成时，SDK 初始化。

**代码示例**

```js
gtSDKModule.initWithRegisterTask("startRegisterTask", "startValidateTask", 5.0);
```

- 接口参数说明：

参数|类型|说明
---|---|---
registerTask|string|实现 api1 请求方法的方法名
validateTask|string|实现 api2 校验方法的方法名
timeout|number|超时时间，单位:`s`，默认`5`

api1、api2 请求如下：

```js
startRegisterTask() {
	// 通过 api1 请求拉取 gt、challenge、success 参数
	let timestamp = new Date().getTime();
	let url = "http://www.geetest.com/demo/gt/register-test?t=" + timestamp;
	uni.request({
		url: url,
		success: (res) => {
			console.log("=========== async task api1 result ======= ");
		    console.log(res.data);
			let gt = res.data.gt;
			let challenge = res.data.challenge;
			let success = res.data.success;
			if (null != gt && null != challenge && null != success) {
				gtSDKModule.setAsyncTaskRegisterResult({
					"gt": gt,
					"challenge": challenge,
					"success": success
				});
			} else {
				gtSDKModule.setAsyncTaskRegisterResult({
					"error": "invalidate response from api1"
				});
			}
		}, 
		fail: (error) => {
			console.log("=========== async task api1 error ======= ");
			console.log(error);
			gtSDKModule.setAsyncTaskRegisterResult({
				"error": error.description
			});
		}
	});
},
startValidateTask(result) {
	// 行为验证弹窗上手动操作验证通过之后，进行二次验证
	console.log("=========== start async validate task ======= ");
	if (typeof result === 'string') {
		console.log("=========== async register task result: " + result + " ======= ");
		let json = JSON.parse(result);
		console.log("=========== json's type: ======= " + typeof json);
		var postData = "";
		for (var key in json) {
			if (key == "result") {
				let jsonResult = json.result;
				console.log("=========== jsonResult: " + JSON.stringify(jsonResult) + " =======");
				for (var jsonKey in jsonResult) {
					postData += (jsonKey + "=" + jsonResult[jsonKey] + "&");
				}
			} else {
				postData += (key + "=" + json[key] + "&");
			}
		}
		if (postData.length > 1) {
			postData = postData.substring(0, postData.length - 1);
		}
		console.log("=========== postData: " + postData + " =======");
		uni.request({
			url: "http://www.geetest.com/demo/gt/validate-test",
			data: postData,
			header: {"Content-Type": "application/x-www-form-urlencoded;charset=UTF-8"},
			method: "POST",
			success: (res) => {
				console.log("=========== async task api2 result ======= ");
				console.log("=========== " + res.data + " =======");
				let status = res.data.status;
				let info = res.data.info;
				console.log("=========== status: " + status + ", info: " + info + " =======");
				// 判断是否验证成功
				if (status == "success") {
					// 将二次验证结果回传给插件
					gtSDKModule.setAsyncTaskValidateResult({
						"result": "true"
					});
				} else {
					// 将二次验证结果回传给插件
					gtSDKModule.setAsyncTaskValidateResult({
						"result": "fasle",
						"error": info
					});
				}
			},
			fail: (error) => {
				console.log("=========== async task api2 error ======= ");
				console.log(error);
				// 将二次验证结果回传给插件
				gtSDKModule.setAsyncTaskValidateResult({
					"result": "false",
					"error": error.description
				});
			}
		});
	} else {
		gtSDKModule.setAsyncTaskValidateResult({
			"result": "false",
			"error": "api2 validate failed"
		});
	}
}
```

### 回传 api1 请求结果给插件

**方法描述**

异步任务方式集成时，api1 请求完成之后，将结果回传给插件。

**代码示例**

```js
let gt = res.data.gt;
let challenge = res.data.challenge;
let success = res.data.success;
if (null != gt && null != challenge && null != success) {
	gtSDKModule.setAsyncTaskRegisterResult({
		"gt": gt,
		"challenge": challenge,
		"success": success
	});
} else {
	gtSDKModule.setAsyncTaskRegisterResult({
		"error": "invalidate response from api1"
	});
}
```

- 接口参数说明：

参数|类型|说明
---|---|---
result|json|api1 接口返回参数


### 回传 api2 请求结果给插件

**方法描述**

异步任务方式集成时，api2 请求完成之后，将结果回传给插件。

**代码示例**

```js
let status = res.data.status;
let info = res.data.info;
console.log("=========== status: " + status + ", info: " + info + " =======");
// 判断是否验证成功
if (status == "success") {
	// 将二次验证结果回传给插件
	gtSDKModule.setAsyncTaskValidateResult({
		"result": "true"
	});
} else {
	// 将二次验证结果回传给插件
	gtSDKModule.setAsyncTaskValidateResult({
		"result": "fasle",
		"error": info
	});
}
```

- 接口参数说明：

参数|类型|说明
---|---|---
result|json|api2 校验结果

校验成功时，参数格式为：

```json
{"result": "true"}
```

校验失败时，参数格式为：

```json
{"result": "false", "error": error.description}
```

### 设置显示二次验证结果的方法

**方法描述**

异步任务方式集成时，api2 请求完成之后，通过设置的方法显示 api2 校验结果，该方法为可选方法，api2 的校验结果可以直接在 `startGTCaptchaWithAnimated` 方法的回调中进行显示。

**代码示例**

```js
// 二次验证成功之后，显示二次验证结果
gtSDKModule.setShowTaskValidateResult("showAsyncTaskValidateResult");

showAsyncTaskValidateResult(result) {
	// 二次验证结果展示
	console.log("=========== async validate task result: " + result + " ======= ");
	if (result.length > 0) {
		uni.showToast({
			icon: "none",
			title: result,
			duration: 2000
		});
	} else {
		uni.showToast({
			icon: "none",
			title: "验证成功",
			duration: 2000
		});
	}
}
```

- 接口参数说明：

参数|类型|说明
---|---|---
result|json|api2 校验结果

参数格式如下：

```json
{"status":"success","info":"登录成功"}
```