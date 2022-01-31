#### Reasons of SSL/TLS failure? 
#### Why would the SSL verification fail?
SSL verification to the backend server will fail for one or many of the following scenarios:
1.	If the backend server certificate’s Common Name (CN) does not match with the Hostname entered in HTTP Settings
2.	If the backend server does not present the complete chain of the certificate during TLS handshake.
a.	This means that, in the Server Hello when the server exchanges the certificate, it should contain the complete chain including Root --> Intermediate (if applicable) --> Leaf. And each one of certificate in the chain should be issue by its parent.
3.	If the Trusted Root Certificate uploaded is invalid/corrupt or expired
4.	If the uploaded Trusted Root Certificate does not match with the Root Certificate of the backend server certificate

