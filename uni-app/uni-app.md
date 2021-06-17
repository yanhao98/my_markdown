# 条件编译

- \#ifdef：if defined 仅在某平台存在
- \#ifndef：if not defined 除了某平台均存在
- **%PLATFORM%**：平台名称

**%PLATFORM%** **可取值如下：**

| 值            | 平台                                                   | 参考文档                                                     |
| :------------ | :----------------------------------------------------- | :----------------------------------------------------------- |
| APP-PLUS      | 5+App                                                  | [HTML5+ 规范](http://www.html5plus.org/doc/)                 |
| APP-PLUS-NVUE | 5+App nvue                                             | [Weex 规范](https://weex.apache.org/cn/guide/)               |
| H5            | H5                                                     |                                                              |
| MP-WEIXIN     | 微信小程序                                             | [微信小程序](https://developers.weixin.qq.com/miniprogram/dev/api/) |
| MP-ALIPAY     | 支付宝小程序                                           | [支付宝小程序](https://docs.alipay.com/mini/developer/getting-started) |
| MP-BAIDU      | 百度小程序                                             | [百度小程序](https://smartprogram.baidu.com/docs/develop/tutorial/codedir/) |
| MP-TOUTIAO    | 头条小程序                                             | [头条小程序](https://developer.toutiao.com/docs/framework/)  |
| MP-QQ         | QQ小程序                                               | （目前仅cli版支持）                                          |
| MP            | 微信小程序/支付宝小程序/百度小程序/头条小程序/QQ小程序 |                                                              |

```js
// #ifdef  **%PLATFORM%**
平台特有的API实现
// #endif
```



# [CSS变量](https://uniapp.dcloud.io/frame?id=css变量)

uni-app 提供内置 CSS 变量

| CSS变量             | 描述                   | 5+App                                                        | 小程序 | H5                   |
| :------------------ | :--------------------- | :----------------------------------------------------------- | :----- | :------------------- |
| --status-bar-height | 系统状态栏高度         | [系统状态栏高度](http://www.html5plus.org/doc/zh_cn/navigator.html#plus.navigator.getStatusbarHeight)、nvue注意见下 | 25px   | 0                    |
| --window-top        | 内容区域距离顶部的距离 | 0                                                            | 0      | NavigationBar 的高度 |
| --window-bottom     | 内容区域距离底部的距离 | 0                                                            | 0      | TabBar 的高度        |



# 交互

## [uni.showLoading(OBJECT)](https://uniapp.dcloud.io/api/ui/prompt?id=showloading)

| 参数     | 类型     | 必填 | 说明                                             | 平台差异说明                  |
| :------- | :------- | :--- | :----------------------------------------------- | :---------------------------- |
| title    | String   | 是   | 提示的内容                                       |                               |
| mask     | Boolean  | 否   | 是否显示透明蒙层，防止触摸穿透，默认：false      | 5+App、微信小程序、百度小程序 |
| success  | Function | 否   | 接口调用成功的回调函数                           |                               |
| fail     | Function | 否   | 接口调用失败的回调函数                           |                               |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |                               |

[uni.hideLoading](https://uniapp.dcloud.io/api/ui/prompt?id=hideloading)

```js
 uni.showLoading({ title: '正在提交', mask: true })

uni.hideLoading()
```



## [uni.showModal(OBJECT)](https://uniapp.dcloud.io/api/ui/prompt?id=showmodal)

| 参数         | 类型     | 必填 | 说明                                                         | 平台差异说明               |
| :----------- | :------- | :--- | :----------------------------------------------------------- | :------------------------- |
| title        | String   | 是   | 提示的标题                                                   |                            |
| content      | String   | 是   | 提示的内容                                                   |                            |
| showCancel   | Boolean  | 否   | 是否显示取消按钮，默认为 true                                |                            |
| cancelText   | String   | 否   | 取消按钮的文字，默认为"取消"，最多 4 个字符                  |                            |
| cancelColor  | HexColor | 否   | 取消按钮的文字颜色，默认为"#000000"                          | H5、微信小程序、百度小程序 |
| confirmText  | String   | 否   | 确定按钮的文字，默认为"确定"，最多 4 个字符                  |                            |
| confirmColor | HexColor | 否   | 确定按钮的文字颜色，H5平台默认为"#007aff"，微信小程序平台默认为"#3CC51F"，百度小程序平台默认为"#3c76ff" | H5、微信小程序、百度小程序 |
| success      | Function | 否   | 接口调用成功的回调函数                                       |                            |
| fail         | Function | 否   | 接口调用失败的回调函数                                       |                            |
| complete     | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |                            |

```js
uni.showModal({
  title: '',
  content: '',
  success: e => {
    if (e.confirm) {
    }
  }
})
```



## [uni.showToast(OBJECT)](https://uniapp.dcloud.io/api/ui/prompt?id=showtoast)

**OBJECT参数说明**

| 参数     | 类型     | 必填 | 说明                                                         | 平台差异说明                      |
| :------- | :------- | :--- | :----------------------------------------------------------- | :-------------------------------- |
| title    | String   | 是   | 提示的内容，长度与 icon 取值有关。                           |                                   |
| icon     | String   | 否   | 图标，有效值详见下方说明。                                   | success/loading/none              |
| image    | String   | 否   | 自定义图标的本地路径                                         | 5+App、H5、微信小程序、百度小程序 |
| mask     | Boolean  | 否   | 是否显示透明蒙层，防止触摸穿透，默认：false                  | 5+App、微信小程序                 |
| duration | Number   | 否   | 提示的延迟时间，单位毫秒，默认：1500                         |                                   |
| position | String   | 否   | 纯文本轻提示显示位置，填写有效值后只有 `title` 属性生效， 有效值详见下方说明。 | 5+App                             |
| success  | Function | 否   | 接口调用成功的回调函数                                       |                                   |
| fail     | Function | 否   | 接口调用失败的回调函数                                       |                                   |
| complete | Function | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |                                   |

```js
uni.showToast({ icon: 'none', title: '' })
```



## [uni.showActionSheet(OBJECT)](https://uniapp.dcloud.io/api/ui/prompt?id=showactionsheet)

| 参数      | 类型          | 必填 | 说明                                             | 平台差异说明                            |
| :-------- | :------------ | :--- | :----------------------------------------------- | :-------------------------------------- |
| itemList  | Array<String> | 是   | 按钮的文字数组                                   | 微信、百度、头条小程序数组长度最大为6个 |
| itemColor | HexColor      | 否   | 按钮的文字颜色，字符串格式，默认为"#000000"      | 头条小程序不支持                        |
| success   | Function      | 否   | 接口调用成功的回调函数，详见返回参数说明         |                                         |
| fail      | Function      | 否   | 接口调用失败的回调函数                           |                                         |
| complete  | Function      | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |                                         |

```js
uni.showActionSheet({
				title:'标题',
				itemList: ['item1', 'item2', 'item3', 'item4'],
				success: (e) => {
					console.log(e.tapIndex);
					uni.showToast({
						title:"点击了第" + e.tapIndex + "个选项",
						icon:"none"
					})
				}
			})
```


# NativeUI

## plus.nativeUI.[confirm](https://www.html5plus.org/doc/zh_cn/nativeui.html#plus.nativeUI.confirm)

```js
void plus.nativeUI.confirm(message, confirmCB, styles);
void plus.nativeUI.confirm(message, confirmCB, title, buttons);	// deprecated
```

```js
plus.nativeUI.confirm("只有标题", null, { "buttons": ["确定"] });
```



## plus.nativeUI.[showWaiting](https://www.html5plus.org/doc/zh_cn/nativeui.html#plus.nativeUI.showWaiting)

```js
Waiting plus.nativeUI.showWaiting(title, styles);
```



# 页面跳转

| API                                                          | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [uni.navigateTo](https://uniapp.dcloud.io/api/router?id=navigateto) | 保留当前页面，跳转到应用内的某个页面，使用uni.navigateBack可以返回到原页面 |
| [uni.redirectTo](https://uniapp.dcloud.io/api/router?id=redirectto) | 关闭当前页面，跳转到应用内的某个页面                         |
| [uni.reLaunch](https://uniapp.dcloud.io/api/router?id=relaunch) | 关闭所有页面，打开到应用内的某个页面                         |
| [uni.switchTab](https://uniapp.dcloud.io/api/router?id=switchtab) | 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面             |
| [uni.navigateBack](https://uniapp.dcloud.io/api/router?id=navigateback) | 关闭当前页面，返回上一页面或多级页面                         |

```js
uni.navigateTo({
    url: 'test?id=1&name=uniapp'
});
```



# 回调数据给上一页

```js
  if (this.callbackKey) {
  this.$helper.prePage()[this.callbackKey] = { addressId: '7', consignee: '梁秀珍', mobile: '13874680558', provinceName: '湖南省', cityName: '永州市', districtName: '江华瑶族自治县', address: '连三桥' };
  uni.navigateBack()
}
```

# 操作代码

## 修改globalData

```js
 getApp().globalData.shoukuanParams.creditCardId = creditCardId;
```

## 隐藏键盘

```js
uni.hideKeyboard();
```

# 生命周期

[https://uniapp.dcloud.io/collocation/frame/lifecycle?id=%e9%a1%b5%e9%9d%a2%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=页面生命周期)

