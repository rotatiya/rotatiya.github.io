Below is the holistic view of troubleshooting network connectivity issue over a web (http) with below two possible ways
1. Using FQDN
2. Using IP Addresss
* #### FQDN 
If you're using FQDN then ensure its resolving to IP address because machine can only communiate over IP Address depends on the type of network you'tr trying to connect over. Network could be either over internet or over intranet but in both scenarios it needs an IP address.

* #### IP
Private IP addresses are used within private networks and are not routable on the internet. Here are the ranges for IPv4 private addresses:
1. Class A: 10.0.0.0 to 10.255.255.255
2. Class B: 172.16.0.0 to 172.31.255.255
3. Class C: 192.168.0.0 to 192.168.255.255

#### Network tools
The available commands to troubleshoot network connectivity issues over web are:
1. nslookup or nameresolver : to validate the dns resolution and to get ip address.
2. tcpping: to test tcp connection this is inbuilt installed library.
3. psping: this is part of windows sysinternals. You can download it and use it to test test connection. 
   
