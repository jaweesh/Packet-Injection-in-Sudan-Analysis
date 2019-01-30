TL;DR
All issues discussed here ARE a result of caching servers that served older versions of software over insecure protocols and channels.

# Shady downloads and redirections

To analyze samples downloaded over insecure channel and secure channels from Sudanese ISPs.




## [Update] January 2019 

Some people started referencing this repo as a solid proof that the Sudanese government is using the APT vulnerability 
CVE-2019-3462 \(https://security-tracker.debian.org/tracker/CVE-2019-3462\) 
and maybe others like 
CVE-2016-1252 (https://security-tracker.debian.org/tracker/CVE-2016-1252\) 
to run malicous payloads. I do not support this theory without a solid proof and I don't have one.
**SO DON'T REFERENCE THIS REPO AS A PROOF FOR THE USAGE OF APT-GET VULNERABILITY BY SUDANESE GOVT.**

Someone (Thank you!) pointed out to me that the hashes for the Comodo antivirus sample I had are the ligit ones, only old vs new hash difference which is a valid claim i can confirm
```
#md5sum cav_installer-original.exe cav_installer-intercepted.exe
7ac6a0bd1c5c0513b2a0bd800a52d084  cav_installer-original.exe
2a2cc463a03efd593ed0da875227cee4  cav_installer-intercepted.exe
```

this hash ``` 7ac6a0bd1c5c0513b2a0bd800a52d084 ``` dates to 22 feb 2018 \(the file downloaded over VPN hence it was over HTTPS and a caching proxy if any couldn't cache it\)
\(https://malwaretips.com/threads/comodo-internet-security-v10-2-0-6514-released.80123/\)

while this hash ``` 2a2cc463a03efd593ed0da875227cee4 ``` dates to 24 Nov 2017 \(the file that got redirected to an IP owned by ZAIN ISP, is probably a caching server with an old version\)
\(https://malwaretips.com/threads/comodo-internet-security-v10-0-2-6420-hotfix-released.77556/\)

As I found out there are only 5 different **installed files** between the two **installers** which is a good way to say that these installers had hotfixes.

Moreover, when I ran 
``` 
apt-get update
```
a hash-mismatch error was generated because out of the box my kali linux supports only **HTTP** updates and my ISP servers happily served an older files with differnet hashes.


As I said before, The sole purpose of this repository is just to try and shed some light of what was happening to me and others in Sudan.


## Introduction and Disclaimers

I created this repository to analyze, collect samples and collaborate efforts towards what is now affecting all ISPs in Sudan \(Zain, MTN and Sudani\).

I am not, and will not be held responsible for any illegal actions or misinterpretation of what comes next in the following analysis, this project started to shed some light on what is going on with internet usage and freedom in my country and nothing else. I am not carrying any illegal actions, any thing will be mentioned here is readily available on the internet with the respected mentioned sources.


The story begins when noticing a mismatch hash when running `apt-get update` on most linux distributions, the issue was more clear when WhatsApp used to download APK files over HTTP, and the connection gets redirected and tampered with resulting in a different APK in size and hash when downloaded over a VPN connection. However since WhatsApp started using HTTPS for downloading and shifted to Google Play and Apple Store, we started collecting samples from AntiVirus software that is downloaded over un-encrpted channels \(HTTP\) for comparison purposes.



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

when updating linux repositories one notices a hash mismatch and have to connect to a VPN in order to successfully update the system repositories which indicate that ISPs like MTN, Zain and Sudani have some sort of caching proxies or packet injection on the fly, ~~however some people would say this is a caching proxy which is invalid claim as caching proxies does not tamper with the package, it may cache an older version but will not change a package structure so it is not the case here.~~ 
Indeed I was served an older versions of software.!

running linux update from MTN network results in an mismatch size and ignored (notice the **HTTP** connectins)

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
caching servers IP addresses that I was redirected to
| IP | net | Date reported | notes |
| :--- | :--- | :--- | :--- |
| 172.19.66.104 | PRIVATE IP SPACE | 13-March-2018 | indicates inside MTN network device |
| 41.223.201.247 | Zain |  | IP is down |

Trying to download an Antivirus software over unencrypted HTTP channel results in the same IP address redirection and size mismatch, however when downloading the same file over a VPN results in a proper safe download of the file
**again a cahching server presenting older version**
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
Saving to: ‘cav_installer.exe’

cav_installer.exe                             100%[====================================================================================================>]   5.25M   880KB/s    in 9.4s    

2018-03-13 14:17:14 (570 KB/s) - ‘cav_installer.exe’ saved [5500784/5500784]



sha256sum *
24f071a2cad7c90b31bf6a3d380ac960d582684379b8ebee1359c3a610f04814  cav_installer-over-VPN.exe
2f3effd8f5598264ec0f39f6880ccf46c1cab8168abfe283a70a3ee4c0c8fffd  cav_installer-intercepted.exe
```

![](/assets/2018-03-13_02h50_14.jpg)


an old WhatsApp download from a strange IP address

![](/assets/whatsapp-evill-download.PNG)

## Past stories and leaks

**irrelevant after confirming caching servers**

~~In 2013 reports found that Blue Coat's tools have been used to censor web sites and monitor the communications of dissidents, activists and journalists in Sudan, Iran and Syria. These countries are sanctioned and sales of such devices, technology and systems are prohibited by law but they managed to get them and utilize them.
 ``` https://www.washingtonpost.com/world/national-security/report-web-monitoring-devices-made-by-us-firm-blue-coat-detected-in-iran-sudan/2013/07/08/09877ad6-e7cf-11e2-a301-ea5a8116d211_story.html?utm_term=.01efb1ac0017 ```~~

~~In Sudan, the Citizen Lab identified the Blue Coat devices on the networks of commercial Internet service provider Canar Telecom. The country, which also faces U.S. sanctions, continues to use the Internet to restrict freedom of expression and crack down on journalists. Sudanese Internet service providers have censored Web sites covering sensitive political protests.
``` same source ```~~

~~In 2014 a Citizen Lab report revealed evidence that Hacking Team's RCS (Remote Control System) was being used by the Sudanese government, something the Italian company flat-out denied.
``` https://citizenlab.ca/storage/bluecoat/CitLab-PlanetBlueCoatRedux-FINAL.pdf ```~~

~~In 2015 HackingTeam, an Italian based IT company that sells offensive intrusion and surveillance software to governments, law enforcement agencies and corporations was hacked and 400GB of data including internal emails, nvoices and source code was leaked and WikiLeaks has an indexed files you can lookup. However HackingTeam stated before that it has never done business with Sudan.
On the leak a contract with Sudan valued at 480,000 Euro and dated July 2, 2012 was published as part of the 400GB cache, in addition, a maintenance list named Sudan as a customer, but one that was "not officially supported".
``` https://www.csoonline.com/article/2944333/data-breach/hacking-team-responds-to-data-breach-issues-public-threats-and-denials.html ```~~


~~In 2016 a friend of mine faced the same issue
``` https://twitter.com/el_ammari/status/723555290550013957 ```~~

## Conclusions

What was happening is ISPs was using caching server and serving older versions of any software downlaoded over insecure channels

~~there is some sort of packet interception and modification on the fly for unencrypted download and traffic,~~ however a VPN connection will prevent this from happening.

### How you can help?

Please, if you live in Sudan and can provide me with a network capture or a memory dump of running
```
apt-get update
```
while you get a hash mismatch error I would be sure of if this is a threat or just a case of noisy caching servers.


