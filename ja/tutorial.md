*[BARK](https://github.com/Finb/Bark)のオープンソースプロジェクトに感謝します*

## プッシュ通知の送信 
1. APPを開き、テストURLをコピーします 

<img src="../_media/example_jp.png" width=365 />

2. 内容を修正し、このURLにリクエストを送信します。<br>
GETまたはPOSTリクエストを送信でき、リクエストが成功すると即座にプッシュ通知を受信します <br>
Barkとの違い：パラメータの優先順位 【POST > GET > URL params】 POSTパラメータはGETパラメータを上書きします（以下同様）

## URL形式
URLはプッシュキー、titleパラメータ、bodyパラメータで構成されています。以下の組み合わせ方法があります

```
https://wzs.app/:key/:body 
https://wzs.app/:key/:title/:body 
https://wzs.app/:key/:title/:subtitle/:body

```

## リクエスト方法
##### GETリクエストのパラメータはURLの後ろに追加します、例：
```sh
curl https://wzs.app/your_key/プッシュ内容?group=グループ&copy=コピー
```
*手動でパラメータをURLに追加する場合は、URLエンコーディングの問題に注意してください。[よくある質問：URLエンコーディング](/faq?id=%e6%8e%a8%e9%80%81%e7%89%b9%e6%ae%8a%e5%ad%97%e7%ac%a6%e5%af%bc%e8%87%b4%e6%8e%a8%e9%80%81%e5%a4%b1%e8%b4%a5%ef%bc%8c%e6%af%94%e5%a6%82-%e6%8e%a8%e9%80%81%e5%86%85%e5%ae%b9%e5%8c%85%e5%90%ab%e9%93%be%e6%8e%a5%ef%bc%8c%e6%88%96%e6%8e%a8%e9%80%81%e5%bc%82%e5%b8%b8-%e6%af%94%e5%a6%82-%e5%8f%98%e6%88%90%e7%a9%ba%e6%a0%bc)を参照してください*

##### POSTリクエストのパラメータはリクエストボディに配置します、例：
```sh
curl -X POST https://wzs.app/your_key \
     -d'body=推送内容&group=分组&copy=复制'
```
##### POSTリクエストはJSONをサポートしています、例：
```sh
curl -X "POST" "//https://wzs.app/your_key" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "body": "Test NoLet Server",
  "title": "Test Title",
  "badge": 1,
  "category": "myNotificationCategory",
  "sound": "minuet.caf",
  "icon": "https://day.app/assets/images/avatar.jpg",
  "group": "test",
  "url": "https://mritd.com"
}'
```

##### JSONリクエストではkeyをリクエストボディに入れることができます、URLパスは/pushである必要があります、例：
```sh
curl -X "POST" "https://wzs.app/push" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "body": "Test NoLet Server",
  "title": "Test Title",
  "device_key": "your_key"
}'
```

## リクエストパラメータ
サポートされているパラメータのリスト、具体的な効果はAPP内でプレビューできます。
すべてのパラメータは様々な書き方に対応しています：SubTitle / subTitle / subtitle / sub_title / sub-title /

| パラメータ | Bark | NoLetでの使用の違い |
| ----- | ----------- | ----------- |
| id | なし | 同じidを持つUUIDを送信すると既存のメッセージを上書きします |
| title | プッシュタイトル | 同じ |
| subtitle | プッシュサブタイトル | 同じ |
| body | プッシュ内容 | 同じ（content/message/data/textもbodyと同等にサポート） |
| markdown | サポートなし | Markdownをレンダリング（mdという略語もサポート） |
| level | プッシュ割り込みレベル。<br>**active**：デフォルト値、システムは即座に画面を点灯して通知を表示します<br>**timeSensitive**：時間指定通知、集中モード中でも通知を表示できます。<br>**passive**：通知を通知リストに追加するだけで、画面点灯による通知はしません。<br>**critical**：重要な通知、集中モードやサイレントモードでも通知します | 互換性あり。パラメータは数字で代用可能：`level=1`<br>0：passive<br>1：active<br>2：timeSensitive<br>3...10：critical、このモードでは数字は音量として使用されます（`level=3...10`） |
| volume | `level=critical`モードでの音量、値の範囲は0...10 | 同じ |
| call | 長い通知、WeChatの電話通知に似ています | 同じ |
| badge | プッシュバッジ、任意の数字が可能 | 未読数に基づいて計算 |
| autoCopy | iOS 14.5以下では自動的にプッシュ内容をコピー、iOS 14.5以上では手動でプッシュを長押しするか下にスワイプする必要があります | このアプリはiOS 16+ |
| copy | プッシュをコピーする際に、コピーする内容を指定します。このパラメータを渡さない場合、プッシュ内容全体がコピーされます。 | 同じ |
| sound | プッシュに異なる着信音を設定できます | アプリ内でデフォルトの着信音を設定可能 |
| icon | プッシュにカスタムアイコンを設定、アイコンは自動的にキャッシュされます | 同じ、さらにクラウドアイコンのアップロードをサポート |
| image | 画像URLを渡すと、スマートフォンがメッセージを受信した後に自動的にダウンロードしてキャッシュします | 同じ |
| savealbum | サポートなし | "1"を渡すと画像を自動的にアルバムに保存します |
| group | メッセージをグループ化し、プッシュは`group`によって通知センターにグループ化して表示されます。<br>また、履歴メッセージリストで異なるグループを選択して表示することもできます。 | 互換性あり |
| isArchive | `1`を渡すとプッシュを保存し、他の値を渡すと保存せず、渡さない場合はアプリ内の設定に従って保存するかどうかを決定します。 | `ttl=日数`を使用 |
| url | プッシュをタップした時にジャンプするURL、URL SchemeとUniversal Linkをサポートしています | 同じ |
