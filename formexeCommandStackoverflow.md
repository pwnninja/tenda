Vendor:Tenda https://www.tendacn.com

Product:Tenda Router AC7/AC9/AC10/AC15/AC18

Version:V15.03.2.13(AC9)    V15.03.05.05(AC15) and earlier

Type:Stack overflow

Author:heshizhi(Wuhan University)

I found a buffer overflow vulnerability in the router's web server--httpd. While processing the "cmdinput" parameter for a post request, the value is directly "strcpy" to a variable on the stack, which overrides the return address of the function. A remote attacker can leak information or hijack program control flow, leading to a RCE.
The details are shown below: 
![image](https://github.com/pwnninja/tenda/blob/main/images/formexeCommandStackoverflow1.png)

POC:
![image](https://github.com/pwnninja/tenda/blob/main/images/formexeCommandStackoverflow2.png)

As you can see in the above figure, if an attacker post data "cmdinput=a*0x300" to /goform/exeCommand, the return address of the function will be overriden and result in a DoS.

