### We will learn on SSL and TLS in little more depth.

###### How to identify if certificate request or certificate you're sharing is client or server certificate.
1) Whenever the client initiate certificate it initiates a SSL connection with EnHANCED KEY USAGE. You can open certificate and inside details of certificate tab myou can find property Enhanced Key Usage. If the enhanced key usage contains 
Client Authentication (1.3.6.1.5.5.7.3.2) => Client certificate please focus on last digit "2"
Server Authentication (1.3.6.1.5.5.7.3.1) => Server certificate please focus on last digit "1"


