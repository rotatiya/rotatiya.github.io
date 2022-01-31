## What is telnet


## Installation
You need to install telnet as bydefault it is disabled. 
#### Windows
On Windows, go to Programs and Features and select Turn Windows features on or off in the Control panel. Once you enabled the telent you’ll need to reboot the system.

#### Mac OS:
On macOS, you can use the nc -v command as a replacement for telnet:
nc -v example.org 80

#### Linux
On most Linux systems, you can install telnet from the package manager.
Plus, the nc command is usually available from a package called netcat.


## How telnet works
When you run it, it’ll clear the screen instead of printing something.
e.g. telnet www.microsoft.com 80

#### HTTP Response codes
- The 100s are informational messages
- The 200s mean you were successful
- The 300s request follow-up action (usually a redirect)
- The 400s mean you sent a bad request
- The 500s mean the server handled the request badly

