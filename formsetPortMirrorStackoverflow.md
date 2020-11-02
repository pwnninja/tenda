Vendor:Tenda https://www.tendacn.com

Product:Tenda Router W18E

Version: V16.01.0.8(1576)_CN and other versions

Type:Stack overflow

Author:heshizhi(Wuhan University)

I found a buffer overflow vulnerability in the router's web server--httpd. While processing the "portMirrorMirroredPorts" parameter for a post request to function formSetPortMirror, the value is directly "sprintf" to a variable on the stack, which overrides the return address of the function. A remote attacker can leak information or hijack program control flow, leading to a RCE.
The details are shown below: 
![image](https://github.com/pwnninja/tenda/blob/main/images/formsetPortMirrorStackoverflow1.png)

POC:
![image](https://github.com/pwnninja/tenda/blob/main/images/formsetPortMirrorStackoverflow2.png)

As you can see in the above figure, if an attacker post data "portMirrorMirroredPorts=1,0,aaaaaa..." to function formSetPortMirror, the return address of the function will be overriden and result in a DoS.
