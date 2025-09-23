
*Thanks to the open source project [BARK](https://github.com/Finb/Bark)*

#### What is Push Encryption

Push encryption is a method to protect push content by encrypting and decrypting the content during sending and receiving using a custom key.<br>This way, the push content cannot be accessed or leaked by NoLet servers or Apple APNs servers during transmission.

#### Setting a Custom Key
1. Open the APP home page
2. Find "Push Encryption" and click on encryption settings
3. 选择加密算法，按要求填写KEY，点击完成保存自定义秘钥

#### 发送加密推送
要发送加密推送，首先需要把 NoLet 请求参数转换成 json 格式的字符串，然后用之前设置的秘钥和相应的算法对字符串进行加密，最后把加密后的密文作为ciphertext参数发送到服务器。<br><br>
**示例：**
```python
# Documentation: "https://wiki.wzs.app"
# python demo: 使用AES加密数据，并发送到服务器
# pip3 install pycryptodome
# 下面只是一种加密的示例，使用时请在app内直接复制

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


# JSON数据
json_string = json.dumps({"body": "test", "sound": "birdsong"})

# 必须32位 这是一个示例
key = b"BxXqdEFEuALb4SGJMQ5zm2fJLrRIz83R"
# IV可以是随机生成的，但如果是随机的就需要放在 iv 参数里传递。
iv= b"BipwZliixOcJDOz8"

# 加密
# 控制台将打印 "Qmlwd1psaWl4T2NKE2CTw4aoH2dGiJ2G0G39EbOK3IiKhxm6URNmqRBDlTh1U1CEoAaeX/zD+vygVi68wnKh3iI="
encrypted_data = encrypt_aes_mode(json_string, key, iv[:12])

# 将加密后的数据转换为Base64编码
encrypted_base64 = base64.b64encode(encrypted_data).decode()

print("加密后的数据（Base64编码", encrypted_base64)

deviceKey = '2uvg28SiADdcrXH46f4xmP'

res = requests.get(f"https://wzs.app/{deviceKey}/test", params = {"ciphertext": encrypted_base64})

print(res.text)

```