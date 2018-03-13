# Packet-Injection-in-Sudan-Analysis

To analyze samples downloaded over insecure channel and secure channels



## Intro and Disclaimers

I created this repository to analyze, collect samples and collaborate efforts towards what is now affecting all ISPs in Sudan \(Zain, MTN and Sudani\).

I am not, and will not be held responsible for any illegal actions or misinterpretation of what comes next in the following analysis, this project started to shed some light on what is going on with internet usage and freedom in my country and nothing else. I am not carrying any illegal actions, any thing will be mentioned here is readily available on the internet with the respected mentioned sources.



The story begins when noticing a mismatch hash when running ` apt-get update` on most linux distributions, the issue was more clear when WhatsApp used to download APK files over HTTP, and the connection gets redirected and tampered with resulting in a different APK in size and hash when downloaded over a VPN connection. However since WhatsApp started using HTTPS for downloading and shifted to Google Play and Apple Store, we started collecting samples from AntiVirus software that is downloaded over unencrpted channels \(HTTP\) for comparison purposes.



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

when updating linux repositories one notices a hash mismatch and have to connect to a VPN in order to successfully update the system repositories which indicate that ISPs like MTN, Zain and Sudani have some sort of packet injection on the fly, however some people would say this is a caching proxy which is invalid claim as chaching proxies does not tamper with the packge, it may cache an older version but will not change a package structure.



