
*[BARK](https://github.com/Finb/Bark)のオープンソースプロジェクトに感謝します*
### APNSインターフェースを直接呼び出す
デバイスのDeviceToken（APPで確認可能）があれば、Apple APNSインターフェースを直接呼び出してデバイスにプッシュ通知を送信できます。APPにサーバーを追加する必要もありません。<br>
以下はコマンドラインでのプッシュ通知送信例です：

```shell
# 设置环境变量
# 下载 key https://github.com/sunvc/NoLetServer/tree/main/deploy/NoLet.p8
# 将 key 文件路径填到下面
TOKEN_KEY_FILE_NAME= 
# 从 app 设置中复制 DeviceToken 到这
DEVICE_TOKEN=

#下面的不要修改
TEAM_ID=FUWV6U942Q
AUTH_KEY_ID=BNY5GUGV38
TOPIC=me.sunvc.Meoworld
APNS_HOST_NAME=api.push.apple.com

# 生成TOKEN
JWT_ISSUE_TIME=$(date +%s)
JWT_HEADER=$(printf '{ "alg": "ES256", "kid": "%s" }' "${AUTH_KEY_ID}" | openssl base64 -e -A | tr -- '+/' '-_' | tr -d =)
JWT_CLAIMS=$(printf '{ "iss": "%s", "iat": %d }' "${TEAM_ID}" "${JWT_ISSUE_TIME}" | openssl base64 -e -A | tr -- '+/' '-_' | tr -d =)
JWT_HEADER_CLAIMS="${JWT_HEADER}.${JWT_CLAIMS}"
JWT_SIGNED_HEADER_CLAIMS=$(printf "${JWT_HEADER_CLAIMS}" | openssl dgst -binary -sha256 -sign "${TOKEN_KEY_FILE_NAME}" | openssl base64 -e -A | tr -- '+/' '-_' | tr -d =)
# 如果有条件，最好改进脚本缓存此 Token。Token 30分钟内复用同一个，每过30分钟重新生成
# 苹果文档指明 TOKEN 生成间隔最短20分钟，TOKEN 有效期最长60分钟
# 间隔过短重复生成会生成失败，TOKEN 超过1小时不重新生成就不能推送
# 但经我不负责任的简单测试可以短时间内正常生成
# 此处仅提醒，或许可能因频繁生成 TOKEN 导致推送失败
AUTHENTICATION_TOKEN="${JWT_HEADER}.${JWT_CLAIMS}.${JWT_SIGNED_HEADER_CLAIMS}"

#发送推送
curl -v --header "apns-topic: $TOPIC" --header "apns-push-type: alert" --header "authorization: bearer $AUTHENTICATION_TOKEN" --data '{"aps":{"alert":"test"}}' --http2 https://${APNS_HOST_NAME}/3/device/${DEVICE_TOKEN}

```

### 推送参数格式
参考 https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification<br>
一定要带上 "mutable-content" : 1 ，否则推送扩展不执行，不会保存推送。<br>

示例：
```js
{
    "aps": {
        "mutable-content": 1,
        "alert": {
            "title" : "title",
            "body": "body"
        },
        "category": "myNotificationCategory",
        "sound": "minuet.caf"
    },
    "icon": "https://day.app/assets/images/avatar.jpg"
}
```