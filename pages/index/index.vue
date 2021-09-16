<template>
	<view class="content">
		<view class="tableTitle">
			<text class="midText">默认集成方式</text>
		</view>
		<view class="button_attr">
			<button type="primary" style="background-color: #8C57E3;" @tap="initGT3Captcha">极验 SDK 初始化</button>
		</view>
		<view class="button_attr">
			<button type="primary" style="background-color: #8C57E3;" @tap="startCaptcha">极验 SDK 开始验证</button>
		</view>
		<view class="tableTitle">
			<text class="midText">异步任务集成方式</text>
		</view>
		<view class="button_attr">
			<button type="primary" style="background-color: #8C57E3;" @tap="initAsyncTaskGT3Captcha">极验 SDK 初始化</button>
		</view>
		<view class="button_attr">
			<button type="primary" style="background-color: #8C57E3;" @tap="startAsyncTaskCaptcha">极验 SDK 开始验证</button>
		</view>
	</view>
</template>

<script>
	var gtSDKModule = uni.requireNativePlugin("Geetest-GT3CaptchaModule"); 
	
	export default {
		onLoad() {
			
		},
		mounted() {
			Vue.prototype.$customAPI1Response = this.customAPI1Response;
			Vue.prototype.$startRegisterTask = this.startRegisterTask;
			Vue.prototype.$startValidateTask = this.startValidateTask;
			Vue.prototype.$showAsyncTaskValidateResult = this.showAsyncTaskValidateResult;
			
			console.log("=========== uni: =========== " + uni);
		},
		methods: {
			initGT3Captcha() {				
				if (gtSDKModule == undefined) {
					console.log("=========== load plugin failed =========== ");
					return;
				}
				console.log("=========== load plugin success ======= ");
				
				let platform = uni.getSystemInfoSync().platform;		
				if (platform == 'android') {
					
					// 传入 api1、api2 地址完成初始化，请首先完成此初始化
					gtSDKModule.initWithAPI1("http://www.geetest.com/demo/gt/register-test", "http://www.geetest.com/demo/gt/validate-test");
					
					// 获取极验 SDK 当前版本号
					let sdkVersion = gtSDKModule.sdkVersion();
					console.log("=========== sdkVersion: =========== " + sdkVersion);
					
					// 设置语言，如果为 null 则使用系统默认语言
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
					gtSDKModule.setLang("zh");
					console.log("=========== setLang: =========== ");
					
					// 设置点击灰色区域是否消失，默认不消失
					gtSDKModule.setCanceledOnTouchOutside(true);
					console.log("=========== setCanceledOnTouchOutside: =========== ");
					
					// 设置服务集群节点
					/*
					* 0为中国集群
					* 1为北美集群，北美集群请联系极验完成相关配置方可使用
					*/
					gtSDKModule.setGt3ServiceNode(0);
					console.log("=========== setGt3ServiceNode: =========== ");
					
					// 设置 webview 答案校验接口超时时间
					gtSDKModule.setWebviewTimeout(9000);
					console.log("=========== setWebviewTimeout: =========== ");
					
			        // 设置 webview 资源加载超时		
					gtSDKModule.setTimeout(6000);
					console.log("=========== setTimeout: =========== ");
					
					// 设置 UI 圆角大小
					gtSDKModule.setCorners(4);
					console.log("=========== setCorners: =========== ");
					
					// 设置是否输出 SD 卡日志
					gtSDKModule.setReleaseLog(true);
					console.log("=========== setReleaseLog: =========== ");
				
				} else if (platform == 'ios') {
					let sdkVersion = gtSDKModule.sdkVersion();
					console.log("=========== sdkVersion: =========== " + sdkVersion);
					
					/** 极验行为验证 SDK 初始化 */
					
					// 打开日志开关，方便调试
					gtSDKModule.setLogEnabled(true);
					console.log("=========== set log enabled =========== ");
					
					// 传入 api1、api2
					gtSDKModule.initWithAPI1("http://www.geetest.com/demo/gt/register-test", "http://www.geetest.com/demo/gt/validate-test", 5.0);
					console.log("=========== init with api1 =========== ");
					
					// 设置语言
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
					gtSDKModule.useLanguage(0);
					console.log("=========== use english language =========== ");
					
					// 设置服务器节点
					/**
					 * 0 - 中国服务集群
					 * 1 - 北美服务集群
					 */
					gtSDKModule.useServiceNode(0);
					console.log("=========== use chinese service node =========== ");
					
					// 设置验证弹窗圆角
					gtSDKModule.useGTViewWithCornerRadius(5.0);
					console.log("=========== corner radius is 5.0 =========== ");
					
					// 设置是否禁用弹窗蒙板交互，为 false 时，点击弹窗蒙板可关闭弹窗，为 true 时，点击弹窗蒙板不能关闭弹窗
					gtSDKModule.disableBackgroundUserInteraction(false);
					console.log("=========== don't disable background user interaction =========== ");
					
					/** 若要修改 api1 的请求或者响应，需要在 registerCaptchaWithCallback 方法前调用 */
					
					// 修改 api1 的请求
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
					
					// 修改 api1 的响应，配合 customAPI1Response 方法完成修改 api1 响应的功能
					gtSDKModule.setModifyAPI1Response(true);
					
					// 注册
					gtSDKModule.registerCaptchaWithCallback((result)=> {
						console.log("=========== register captcha =========== ");
					});
				}
			},
			initAsyncTaskGT3Captcha() {
				if (gtSDKModule == undefined) {
					console.log("=========== load plugin failed =========== ");
					return;
				}
				
				console.log("=========== load plugin success ======= ");
				
				let platform = uni.getSystemInfoSync().platform;		
				if (platform == 'android') {
					
					// 异步集成 api1、2 地址可不传，但此方法请务必调用
					gtSDKModule.initWithAPI1("http://www.geetest.com/demo/gt/register-test", "http://www.geetest.com/demo/gt/validate-test");
					
					// 获取极验 SDK 当前版本号
					let sdkVersion = gtSDKModule.sdkVersion();
					console.log("=========== sdkVersion: =========== " + sdkVersion);
					
					// 设置语言，如果为 null 则使用系统默认语言
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
					gtSDKModule.setLang("en");
					console.log("=========== setLang: =========== ");
					
					// 设置点击灰色区域是否消失，默认不消失
					gtSDKModule.setCanceledOnTouchOutside(true);
					console.log("=========== setCanceledOnTouchOutside: =========== ");
					
					// 设置服务集群节点
					/*
					* 0为中国集群
					* 1为北美集群，北美集群请联系极验完成相关配置方可使用
					*/
					gtSDKModule.setGt3ServiceNode(0);
					console.log("=========== setGt3ServiceNode: =========== ");
					
					// 设置 webview 答案校验接口超时时间
					gtSDKModule.setWebviewTimeout(9000);
					console.log("=========== setWebviewTimeout: =========== ");
					
			        // 设置 webview 资源加载超时		
					gtSDKModule.setTimeout(6000);
					console.log("=========== setTimeout: =========== ");
					
					// 设置 UI 圆角大小
					gtSDKModule.setCorners(4);
					console.log("=========== setCorners: =========== ");
					
					// 设置是否输出 SD 卡日志
					gtSDKModule.setReleaseLog(true);
					console.log("=========== setReleaseLog: =========== ");
					
				} else if (platform == 'ios') {
					let sdkVersion = gtSDKModule.sdkVersion();
					console.log("=========== sdkVersion: =========== " + sdkVersion);
					
					/** 极验行为验证 SDK 初始化 */
					
					// 打开日志开关，方便调试
					gtSDKModule.setLogEnabled(true);
					console.log("=========== set log enabled =========== ");
					
					// 异步任务集成时，api1、api2 可为空
					gtSDKModule.initWithRegisterTask("startRegisterTask", "startValidateTask", 5.0);
					console.log("=========== init with api1 =========== ");
					
					// 设置语言
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
					gtSDKModule.useLanguage(0);
					console.log("=========== use english language =========== ");
					
					// 设置服务器节点
					/**
					 * 0 - 中国服务集群
					 * 1 - 北美服务集群
					 */
					gtSDKModule.useServiceNode(0);
					console.log("=========== use chinese service node =========== ");
					
					// 设置验证弹窗圆角
					gtSDKModule.useGTViewWithCornerRadius(5.0);
					console.log("=========== corner radius is 5.0 =========== ");
					
					// 设置是否禁用弹窗蒙板交互，为 false 时，点击弹窗蒙板可关闭弹窗，为 true 时，点击弹窗蒙板不能关闭弹窗
					gtSDKModule.disableBackgroundUserInteraction(true);
					console.log("=========== disable background user interaction =========== ");
					
					// 二次验证成功之后，显示二次验证结果
					gtSDKModule.setShowTaskValidateResult("showAsyncTaskValidateResult");
				}
			},
			startCaptcha() {
				if (gtSDKModule == undefined) {
					console.log("=========== load plugin failed =========== ");
					return;
				}
				
				let platform = uni.getSystemInfoSync().platform;
				if (platform == 'android') {
					
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
					
				} else if (platform == 'ios') {
					console.log("=========== did touch start ======= ");
					// 是否使用默认的 api2 请求
					gtSDKModule.setShouldUseDefaultSecondaryValidate(true);
					
					// 处理验证弹窗关闭情况
					gtSDKModule.handleGTViewClose((result) => {
						console.log("=========== gt view is closed ======= ");
					});
					
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
						
					});
				}
			},
			startAsyncTaskCaptcha() {
				if (gtSDKModule == undefined) {
					console.log("=========== load plugin failed =========== ");
					return;
				}
				
				let platform = uni.getSystemInfoSync().platform;
				if (platform == 'android') {
													
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
						// api2CallBack为二次验证请求参数回调
						console.log("=========== captcha js result ======= " + JSON.stringify(api2CallBack));
						this.$options.methods.startValidateTask(JSON.stringify(api2CallBack));
						
					}, (closedResult) => {
						// closedResult为关闭弹框方式回调
						// 1 点击验证码的关闭按钮来关闭验证码, 2 点击屏幕关闭验证码, 3 点击返回键关闭验证码
						console.log("=========== captcha js result ======= " + closedResult.closedNumber);
						
					});
					
					
				} else if (platform == 'ios') {
					// 处理验证弹窗关闭情况
					gtSDKModule.handleGTViewClose((result) => {
						console.log("=========== gt view is closed ======= ");
					});
					
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
				}
			},
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
						gtSDKModule.customAPI1Response(json);
					}
				}
			}, 
			startRegisterTask() {
				console.log("=========== start async register task ======= ");
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
			},
			showAsyncTaskValidateResult(result) {
				// 二次验证结果展示
				console.log("=========== async validate task result: " + result + " ======= ");
				let status = result.status;
				let info = result.info;
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
			}
		}
	}
</script>

<style>
	.result_group {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		background-color: #B9F6CA;
		margin: 0 10upx;
		border-radius: 10upx;

	}

	.action-row {
		display: flex;
		flex-direction: row;
		justify-content: center;
		margin: 0 10upx;
		border-radius: 10upx;
		width: 100%;
	}

	.text_attr {
		width: 95%;
		margin-left: 30upx;
		margin-top: 10upx;
		margin-bottom: 10upx;
		margin-right: 30upx;
		word-wrap: break-word;
		word-break: break-all;
		font-size: 30upx;
	}

	.button_attr {
		justify-content: center;
		margin-top: 30upx;
		margin-left: 15upx;
		margin-right: 15upx;
		margin-bottom: 30upx;
		background-color: #8C57E3;
	}

	.tableTitle {
		position: relative;
		margin-top: 50upx;
		margin-bottom: 50upx;
		width: 100%;
		height: 1px;
		background-color: #d4d4d4;
		text-align: center;
		font-size: 16px;
		color: rgba(101, 101, 101, 1);
	}

	.midText {
		position: absolute;
		left: 50%;
		background-color: #f2f2f5;
		transform: translateX(-50%) translateY(-50%);
	}
</style>
