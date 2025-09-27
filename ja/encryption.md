
*[BARK](https://github.com/Finb/Bark)のオープンソースプロジェクトに感謝します*

#### プッシュ暗号化とは

プッシュ暗号化は、プッシュコンテンツを保護する方法で、送信と受信時にカスタムキーを使用してプッシュコンテンツを暗号化および復号化します。<br>これにより、転送中のプッシュコンテンツがNoLetサーバーやApple APNsサーバーによってアクセスされたり漏洩したりすることはありません。

#### カスタム秘密鍵の設定
1. APPのホームページを開きます
2. 「プッシュ暗号化」を見つけて、暗号化設定をタップします
3. 暗号化アルゴリズムを選択し、要求に従ってKEYを入力し、完了をタップしてカスタム秘密鍵を保存します

#### 暗号化されたプッシュの送信
暗号化されたプッシュを送信するには、まずNoLetリクエストパラメータをJSON形式の文字列に変換し、以前に設定した秘密鍵と対応するアルゴリズムを使用して文字列を暗号化し、最後に暗号化されたテキストをciphertextパラメータとしてサーバーに送信します。<br><br>
**例：**
```python
# ドキュメント: "https://wiki.wzs.app"
# Pythonデモ: AESを使用してデータを暗号化し、サーバーに送信する
# pip3 install pycryptodome
# 以下は暗号化の一例です。使用する際はアプリ内で直接コピーしてください

import json
import base64
import requests
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad


def encrypt_aes_mode(data, key, iv):
	cipher = AES.new(key, AES.MODE_GCM, iv)
	padded_data = data.encode()
	encrypted_data, tag = cipher.encrypt_and_digest(padded_data)
	return iv + encrypted_data + tag


# JSONデータ
json_string = json.dumps({"body": "test", "sound": "birdsong"})

# 32バイト必須 これは例です
key = b"BxXqdEFEuALb4SGJMQ5zm2fJLrRIz83R"
# IVはランダムに生成できますが、ランダムの場合はivパラメータで渡す必要があります
iv= b"BipwZliixOcJDOz8"

# 暗号化
# コンソールには "Qmlwd1psaWl4T2NKE2CTw4aoH2dGiJ2G0G39EbOK3IiKhxm6URNmqRBDlTh1U1CEoAaeX/zD+vygVi68wnKh3iI=" と表示されます
encrypted_data = encrypt_aes_mode(json_string, key, iv[:12])

# 暗号化されたデータをBase64エンコードに変換
encrypted_base64 = base64.b64encode(encrypted_data).decode()

print("暗号化されたデータ（Base64エンコード）:", encrypted_base64)

deviceKey = '2uvg28SiADdcrXH46f4xmP'

res = requests.get(f"https://wzs.app/{deviceKey}/test", params = {"ciphertext": encrypted_base64})

print(res.text)

```