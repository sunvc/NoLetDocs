
*Thanks to the open source project [BARK](https://github.com/Finb/Bark)*

#### What is Push Encryption

Push encryption is a method to protect push content by encrypting and decrypting the content during sending and receiving using a custom key.<br>This way, the push content cannot be accessed or leaked by NoLet servers or Apple APNs servers during transmission.

#### Setting a Custom Key
1. Open the APP home page
2. Find "Push Encryption" and click on encryption settings
3. Select the encryption algorithm, fill in the KEY as required, and click "Done" to save your custom key

#### Sending Encrypted Push Notifications
To send encrypted push notifications, you first need to convert the NoLet request parameters into a JSON format string, then encrypt the string using the previously set key and corresponding algorithm, and finally send the encrypted text as a ciphertext parameter to the server.<br><br>
**Example:**
```python
# Documentation: "https://wiki.wzs.app"
# Python demo: Using AES to encrypt data and send it to the server
# pip3 install pycryptodome
# This is just one encryption example, please copy directly from the app when using

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


# JSON data
json_string = json.dumps({"body": "test", "sound": "birdsong"})

# Must be 32 characters - this is an example
key = b"BxXqdEFEuALb4SGJMQ5zm2fJLrRIz83R"
# IV can be randomly generated, but if random, it needs to be passed in the iv parameter
iv= b"BipwZliixOcJDOz8"

# Encryption
# The console will print "Qmlwd1psaWl4T2NKE2CTw4aoH2dGiJ2G0G39EbOK3IiKhxm6URNmqRBDlTh1U1CEoAaeX/zD+vygVi68wnKh3iI="
encrypted_data = encrypt_aes_mode(json_string, key, iv[:12])

# Convert encrypted data to Base64 encoding
encrypted_base64 = base64.b64encode(encrypted_data).decode()

print("Encrypted data (Base64 encoded):", encrypted_base64)

deviceKey = '2uvg28SiADdcrXH46f4xmP'

res = requests.get(f"https://wzs.app/{deviceKey}/test", params = {"ciphertext": encrypted_base64})

print(res.text)

```