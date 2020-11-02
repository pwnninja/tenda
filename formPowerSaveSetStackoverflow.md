Vendor: Tenda https://www.tendacn.com

Product: Tenda Router AC7/AC9/AC10/AC15/AC18

Version: V15.03.2.13(AC9)  V15.03.05.18(AC15) and earlier

Type: Stack overflow

Author: heshizhi(Wuhan University)

I found a buffer overflow vulnerability in the router's web server--httpd. While processing the "time" parameter for a post request to /goform/PowerSaveSet, the value is directly "sscanf" and "sprintf" to a variable on the stack, which overrides the return address of the function. A remote attacker can leak information or hijack program control flow, leading to a RCE.
The details are shown below: 
![image](https://github.com/pwnninja/tenda/blob/main/images/formPowerSaveSetStackoverflow1.png)

POC:
![image](https://github.com/pwnninja/tenda/blob/main/images/formPowerSaveSetStackoverflow2.png)

As you can see in the above figure, if an attacker post data "time=aaaaaa...-00:02" to /goform/PowerSaveSet, the return address of the function will be overriden and result in a DoS.
