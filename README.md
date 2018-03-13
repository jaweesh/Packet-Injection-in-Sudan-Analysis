# Packet-Injection-in-Sudan-Analysis

To analyze samples downloaded over insecure channel and secure channels

## Intro and Disclaimers

I created this repository to analyze, collect samples and collaborate efforts towards what is now affecting all ISPs in Sudan \(Zain, MTN and Sudani\).

I am not, and will not be held responsible for any illegal actions or misinterpretation of what comes next in the following analysis, this project started to shed some light on what is going on with internet usage and freedom in my country and nothing else. I am not carrying any illegal actions, any thing will be mentioned here is readily available on the internet with the respected mentioned sources.

The story begins when noticing a mismatch hash when running `apt-get update` on most linux distributions, the issue was more clear when WhatsApp used to download APK files over HTTP, and the connection gets redirected and tampered with resulting in a different APK in size and hash when downloaded over a VPN connection. However since WhatsApp started using HTTPS for downloading and shifted to Google Play and Apple Store, we started collecting samples from AntiVirus software that is downloaded over unencrpted channels \(HTTP\) for comparison purposes.

## Organization of the Repo

analysis will be on this page while samples will have their own folder, executables, packet captures and binary diffs will be on the respected folder for each sample.

* Repo
  * samples
    * Comodo antivirus
      * files, description and details
    * network captures
      * files, description and details
  * main analysis page

## Observations and Analysis

when updating linux repositories one notices a hash mismatch and have to connect to a VPN in order to successfully update the system repositories which indicate that ISPs like MTN, Zain and Sudani have some sort of packet injection on the fly, however some people would say this is a caching proxy which is invalid claim as chaching proxies does not tamper with the packge, it may cache an older version but will not change a package structure so it is not the case here.

running update from MTN network results in an mismatch size and ignored

```
apt-get update
Get:1 http://kali-za.bitcrack.net/kali kali-rolling InRelease [30.5 kB]
Get:2 http://kali-za.bitcrack.net/kali kali-rolling/main i386 Packages [15.9 MB]
Get:3 http://kali-za.bitcrack.net/kali kali-rolling/main i386 Contents (deb) [33.9 MB]                                                                                                      
Err:3 http://kali-za.bitcrack.net/kali kali-rolling/main i386 Contents (deb)                                                                                                                
  File has unexpected size (32613856 != 33858927). Mirror sync in progress? [IP: 172.19.66.104 80]
  Hashes of expected file:
   - Filesize:33858927 [weak]
   - SHA256:dbde5770ab080420d319c9e0017bc02acd4d7af859176d4a7eb48a954da77e85
   - SHA1:32a011cc46fa16ac7c266010afc8f25ce3e50a08 [weak]
   - MD5Sum:39c82afd6f4b1bdda0f0f03d2369a3fe [weak]
  Release file created at: Tue, 13 Mar 2018 06:05:18 +0000
Fetched 15.9 MB in 27s (579 kB/s)                                                                                                                                                           
Reading package lists... Done
E: Failed to fetch http://172.19.66.104:80/videoplayer/Contents-i386.gz?ich_u_r_i=5822d70054b7a4f932aa42e6efd1a6e0&ich_s_t_a_r_t=0&ich_e_n_d=0&ich_k_e_y=ad2b5c90b231a53049d2521a25604a0e4db61d405213ad8bbadbfbc1e219b072&ich_t_y_p_e=1&ich_d_i_s_k_i_d=12&ich_s_e_q=2679810&ich_u_n_i_t=1  File has unexpected size (32613856 != 33858927). Mirror sync in progress? [IP: 172.19.66.104 80]
   Hashes of expected file:
    - Filesize:33858927 [weak]
    - SHA256:dbde5770ab080420d319c9e0017bc02acd4d7af859176d4a7eb48a954da77e85
    - SHA1:32a011cc46fa16ac7c266010afc8f25ce3e50a08 [weak]
    - MD5Sum:39c82afd6f4b1bdda0f0f03d2369a3fe [weak]
   Release file created at: Tue, 13 Mar 2018 06:05:18 +0000
E: Some index files failed to download. They have been ignored, or old ones used instead.

```

![](/assets/2018-03-13_14h05_25.jpg)

## IP addresses in question

| IP | net | Date reported | notes |
| :--- | :--- | :--- | :--- |
| 172.19.66.104 | PRIVATE IP SPACE | 13-March-2018 | indicates inside MTN network device |
|  |  |  |  |

Trying to download an Antivirus software over unencrypted HTTP channel results in the same IP address redirection and size mismatch, however when downloading the same file over a VPN results in a proper safe download of the file

```
wget -d  "http://download.comodo.com/cis/download/installs/1000/standalone/cav_installer.exe"
DEBUG output created by Wget 1.19.4 on linux-gnu.

Reading HSTS entries from /root/.wget-hsts
URI encoding = ‘UTF-8’
Converted file name 'cav_installer.exe' (UTF-8) -> 'cav_installer.exe' (UTF-8)
--2018-03-13 14:17:04--  http://download.comodo.com/cis/download/installs/1000/standalone/cav_installer.exe
Resolving download.comodo.com (download.comodo.com)... 178.255.82.5, 2a02:1788:4fd::b2ff:5205
Caching download.comodo.com => 178.255.82.5 2a02:1788:4fd::b2ff:5205
Connecting to download.comodo.com (download.comodo.com)|178.255.82.5|:80... connected.
Created socket 3.
Releasing 0x019330a0 (new refcount 1).

---request begin---
GET /cis/download/installs/1000/standalone/cav_installer.exe HTTP/1.1
User-Agent: Wget/1.19.4 (linux-gnu)
Accept: */*
Accept-Encoding: identity
Host: download.comodo.com
Connection: Keep-Alive

---request end---
HTTP request sent, awaiting response... 
---response begin---
HTTP/1.1 302 Found
Connection: close
Location: http://172.19.66.104:80/videoplayer/cav_installer.exe?ich_u_r_i=61fee96fe236ac22746eb8d0992e0474&ich_s_t_a_r_t=0&ich_e_n_d=0&ich_k_e_y=ad2b5c90b231a53049d2521a25604a0e510bcc0556d2e91988e988dfb864d267&ich_t_y_p_e=1&ich_d_i_s_k_i_d=6&ich_s_e_q=2691841&ich_u_n_i_t=1
Pragma: no-cache

---response end---
302 Found
Location: http://172.19.66.104:80/videoplayer/cav_installer.exe?ich_u_r_i=61fee96fe236ac22746eb8d0992e0474&ich_s_t_a_r_t=0&ich_e_n_d=0&ich_k_e_y=ad2b5c90b231a53049d2521a25604a0e510bcc0556d2e91988e988dfb864d267&ich_t_y_p_e=1&ich_d_i_s_k_i_d=6&ich_s_e_q=2691841&ich_u_n_i_t=1 [following]
Closed fd 3
URI content encoding = None
Converted file name 'cav_installer.exe' (UTF-8) -> 'cav_installer.exe' (UTF-8)
--2018-03-13 14:17:04--  http://172.19.66.104/videoplayer/cav_installer.exe?ich_u_r_i=61fee96fe236ac22746eb8d0992e0474&ich_s_t_a_r_t=0&ich_e_n_d=0&ich_k_e_y=ad2b5c90b231a53049d2521a25604a0e510bcc0556d2e91988e988dfb864d267&ich_t_y_p_e=1&ich_d_i_s_k_i_d=6&ich_s_e_q=2691841&ich_u_n_i_t=1
Connecting to 172.19.66.104:80... connected.
Created socket 3.
Releasing 0x01934090 (new refcount 0).
Deleting unused 0x01934090.

---request begin---
GET /videoplayer/cav_installer.exe?ich_u_r_i=61fee96fe236ac22746eb8d0992e0474&ich_s_t_a_r_t=0&ich_e_n_d=0&ich_k_e_y=ad2b5c90b231a53049d2521a25604a0e510bcc0556d2e91988e988dfb864d267&ich_t_y_p_e=1&ich_d_i_s_k_i_d=6&ich_s_e_q=2691841&ich_u_n_i_t=1 HTTP/1.1
User-Agent: Wget/1.19.4 (linux-gnu)
Accept: */*
Accept-Encoding: identity
Host: 172.19.66.104
Connection: Keep-Alive

---request end---
HTTP request sent, awaiting response... 
---response begin---
HTTP/1.1 200 OK
Server: httpserver
Date: Tue, 13 Mar 2018 11:17:42 GMT
Content-Type: application/octet-stream
Connection: keep-alive
Content-Length: 5500784
Last-Modified: Fri, 24 Nov 2017 08:44:30 GMT
Content-Disposition: attachment; filename="cav_installer.exe"
Expires: Tue, 13 Mar 2018 11:17:42 GMT
Cache-Control: max-age=0
Accept-Ranges: bytes

---response end---
200 OK
Registered socket 3 for persistent reuse.
Length: 5500784 (5.2M) [application/octet-stream]
Saving to: ‘cav_installer.exe.2’

cav_installer.exe.2                             100%[====================================================================================================>]   5.25M   880KB/s    in 9.4s    

2018-03-13 14:17:14 (570 KB/s) - ‘cav_installer.exe.2’ saved [5500784/5500784]



sha256sum *
24f071a2cad7c90b31bf6a3d380ac960d582684379b8ebee1359c3a610f04814  cav_installer-over-VPN.exe
2f3effd8f5598264ec0f39f6880ccf46c1cab8168abfe283a70a3ee4c0c8fffd  cav_installer-intercepted.exe
```

![](/assets/2018-03-13_02h50_14.jpg)

## Conclusions



