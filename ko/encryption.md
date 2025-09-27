
*[BARK](https://github.com/Finb/Bark)의 오픈 소스 프로젝트에 감사드립니다*

#### 푸시 암호화란 무엇인가요

푸시 암호화는 푸시 내용을 보호하는 방법으로, 사용자 정의 키를 사용하여 전송 및 수신 시 푸시 내용을 암호화하고 복호화합니다.<br>이렇게 하면 푸시 내용이 전송 과정에서 NoLet 서버와 애플 APNs 서버에 의해 획득되거나 유출되지 않습니다.

#### 사용자 정의 키 설정하기
1. 앱 홈페이지를 엽니다
2. "푸시 암호화"를 찾아 암호화 설정을 클릭합니다
3. 암호화 알고리즘을 선택하고 요구 사항에 따라 KEY를 입력한 후 완료를 클릭하여 사용자 정의 키를 저장합니다

#### 암호화된 푸시 보내기
암호화된 푸시를 보내려면 먼저 NoLet 요청 매개변수를 json 형식의 문자열로 변환한 다음, 이전에 설정한 키와 해당 알고리즘을 사용하여 문자열을 암호화하고, 마지막으로 암호화된 텍스트를 ciphertext 매개변수로 서버에 전송합니다.<br><br>
**예시:**
```python
# Documentation: "https://wiki.wzs.app"
# python 데모: AES를 사용하여 데이터를 암호화하고 서버로 전송
# pip3 install pycryptodome
# 아래는 암호화의 한 예시일 뿐이며, 사용 시 앱 내에서 직접 복사하세요

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


# JSON 데이터
json_string = json.dumps({"body": "test", "sound": "birdsong"})

# 반드시 32비트여야 합니다. 이것은 예시입니다
key = b"BxXqdEFEuALb4SGJMQ5zm2fJLrRIz83R"
# IV는 무작위로 생성될 수 있지만, 무작위인 경우 iv 매개변수에 전달해야 합니다.
iv= b"BipwZliixOcJDOz8"

# 암호화
# 콘솔에 "Qmlwd1psaWl4T2NKE2CTw4aoH2dGiJ2G0G39EbOK3IiKhxm6URNmqRBDlTh1U1CEoAaeX/zD+vygVi68wnKh3iI="가 출력됩니다
encrypted_data = encrypt_aes_mode(json_string, key, iv[:12])

# 암호화된 데이터를 Base64로 인코딩
encrypted_base64 = base64.b64encode(encrypted_data).decode()

print("암호화된 데이터(Base64 인코딩)", encrypted_base64)

deviceKey = '2uvg28SiADdcrXH46f4xmP'

res = requests.get(f"https://wzs.app/{deviceKey}/test", params = {"ciphertext": encrypted_base64})

print(res.text)
```