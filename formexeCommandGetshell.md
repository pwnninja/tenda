Vendor:Tenda https://www.tendacn.com

Product:Tenda Router AC7/AC9/AC10/AC15/AC18

Version:V15.03.2.13(AC9)    V15.03.05.05(AC15) and earlier

Type:OS Command Injection

Author:heshizhi(Wuhan University)

I found an OS command injection vulnerability in the router's web server--httpd. While processing the "cmdinput" parameter for a post request to /goform/exeCommand, the value is passed to function doSystemCmd.
A remote attacker can post "cmdinput=ls;telnetd -p 9090;", which will open a shell on port 9090. The router will be compromised easily. The attacker can also execute other commands in this way, such as "reboot".

The details are shown below: 
![image](https://github.com/pwnninja/tenda/blob/main/images/formexeCommandGetshell1.png)

POC:
![image](https://github.com/pwnninja/tenda/blob/main/images/formexeCommandGetshell2.png)
![image](https://github.com/pwnninja/tenda/blob/main/images/formexeCommandGetshell3.png)
As you can see in the above figure, if an attacker post data "cmdinput=ls;telnetd -p 9090;" to /goform/exeCommand, he will get a shell on port 9090 directly.


