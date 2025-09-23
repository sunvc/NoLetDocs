*Thanks to the open source project [BARK](https://github.com/Finb/Bark)*
#### How Privacy Can Be Leaked <!-- {docsify-ignore-all} -->

The route of a push notification from sending to receiving is:<br>
Sender <font color='red'> →Server①</font> → Apple APNS Server → Your Device → <font color='red'>NoLet APP②</font>.

Privacy may be leaked at the two red points <br>
* The sender does not use HTTPS or uses a public server *(the author can see request logs)*
* The NoLet App itself is not secure, and the version uploaded to the App Store has been modified.

#### 解决服务端隐私问题
* 你可以使用开源的后端代码，自行[部署后端服务](/deploy.md)，开启HTTPS。
* 使用自定义秘钥的[加密推送](/encryption) ，加密推送内容

#### 保证 APP 完全由开源代码构建
为确保 App 是安全、未经任何人（包含作者）修改过的，NoLet 是由 Apple Xcode Cloud 构建后上传到 App Store。

*这里不考虑iOS是否泄露隐私*