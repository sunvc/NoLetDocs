
*Thanks to the open source project [BARK](https://github.com/Finb/Bark)*

#### Unable to Receive Push Notifications
Check if the Device Token is normal in the App settings. If not, refer to [here](#Device-Token-Shows-Unknown)<br/>
If it's normal, try restarting your device. If you still can't receive push notifications, check if the push request returns status code 200.<br/>
If all checks are normal but you still have issues, you can provide feedback in the [NoLet Feedback Group](https://t.me/PushToMe).

#### Device Token Shows Unknown
This is likely because your device is not properly connected to Apple servers, accompanied by issues such as iMessage unavailability and inability to receive push notifications from other apps.<br/>
可以尝试切换网络、重启手机、如果翻墙代理了 Apple 服务可以关闭翻墙工具。<br/>
此问题是用户设备与苹果服务器的连接问题，作者并不能提供任何帮助，需自己尝试解决。

#### 推送使用次数限制
正常请求（HTTP状态码为200）无任何限制。<br>
但如果在5分钟内超过1000次错误请求（HTTP状态码为400 404 500）<b>IP会被 BAN 24小时</b> 

#### 莫名收到未知推送，比如 NoContent
可能的原因：<br>
1. 如果用 Safari 发送过推送，在Safari输入任意网址时，可能 Safari 对历史记录搜索进行自动补全时，正好补全成 Bark API 的 URL，然后预加载触发推送。
2. 如果将 Bark API URL 发送到聊天软件如微信文件传输助手，微信会不定时的请求 URL 触发推送。
3. 推送 Key 泄露，推荐在服务器列表页面重置 Key。

#### 时效性通知无效 
可以尝试<b>重启设备</b>来解决。

#### 无法保存通知历史，或下拉推送没有点击复制按钮无法复制
可以尝试<b>重启设备</b>来解决。<br />
因某些原因导致推送服务扩展（[UNNotificationServiceExtension](https://developer.apple.com/documentation/usernotifications/unnotificationserviceextension)）未能正常运行，执行通知保存的代码未能正常执行。

#### 自动复制推送失效
iOS 14.5 之后的版本因权限收紧，不能在收到推送时自动复制推送内容到剪切板。<br/>
可暂时先下拉推送或在锁屏界面左滑推送点查看即可自动复制，或点击弹出的推送复制按钮。

#### 默认打开通知历史列表
再次开启APP时，会跳转到上次打开的页面。<br />
只需退出APP时，停留在历史消息页面，再次打开APP时就是历史消息页面。

#### 推送 API 是否支持 POST 请求？
NoLet支持 GET POST ,支持使用Json<br>
无论哪种请求方式，参数名都一样, 参考[使用教程](/tutorial#请求方式)

#### 推送特殊字符导致推送失败，比如 推送内容包含链接，或推送异常 比如 + 变成空格
这是因为整个链接不规范导致的问题，常发生在自己手动拼接URL时。<br>
拼接URL时，注意将参数进行URL编码 

```sh
# 例如
https://wzs.app/key/{推送内容}

# 如果{推送内容}是
"a/b/c/"

# 则最后拼接的URL是
https://wzs.app/key/a/b/c/
# 将找不到对应的路由，后端程序将返回404

# 应该将 {推送内容} url编码后再进行拼接
https://wzs.app/key/a%2Fb%2Fc%2F
```
如果是使用成熟的HTTP库时，参数都会被自动处理，无需自己手动编码。<br>
但如果是自己去拼接URL时，则需要特别注意参数中的特殊字符，**最好不管有没有特殊字符，无脑套一层URL编码**。

#### 如何保障隐私安全
参考[隐私安全](/privacy)
