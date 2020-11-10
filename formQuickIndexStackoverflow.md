Vendor: Tenda https://www.tendacn.com

Product: Tenda Router AC7/AC9/AC10/AC15/AC18

Version: V15.03.2.13(AC9)  V15.03.05.18(AC15) and earlier

Type: Stack overflow

Author: heshizhi(Wuhan University)

I found a buffer overflow vulnerability in the router's web server--httpd. While processing the "PPPOEPassword" parameter for a post request to /goform/QuickIndex, the value is copied to a variable on the stack via function sub_4F7C0, which overrides the return address of the function. A remote attacker can leak information or hijack program control flow, leading to a RCE.
The details are shown below: 
![image](https://github.com/pwnninja/tenda/blob/main/images/formQuickIndexStackoverflow1.png)

In function sub_4F7C0, string a1 copied to a2 without security check:
![image](https://github.com/pwnninja/tenda/blob/main/images/formQuickIndexStackoverflow2.png)

POC:
![image](https://github.com/pwnninja/tenda/blob/main/images/formQuickIndexStackoverflow3.png)

As you can see in the above picture, if an attacker post data "PPPOEPassword=aaaaaa..." to /goform/PowerSaveSet, the return address of the function will be overriden and result in a DoS.Futhermore, the attacker can exploit this vulnerability, using ROP to hijack control flow and execute arbitrary code on router with root privilege.

In the next picture, you can see the pid of httpd has been changed. That indicates a DoS has happened, causing the router to restart httpd:

![image](https://github.com/pwnninja/tenda/blob/main/images/formQuickIndexStackoverflow4.png)
