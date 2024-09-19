# This article is about a bug that impact Windows Servers OS that uses a GPO to define the DNS suffix searchlist.
*Impacted versions: Windows Server 2012 and 2016. Not tested on newer versions.*
*Non-impacted versions: Windows Pro*

## 1. Issue context and how to reproduce it.
This bug appeared while playing with domain relationship infrastructure, but can be reproced without any relationship.
It appeared when trying to ping NETBIOS name of computer of another domain name.  

> On the following scheme: red domain uses manual suffix searchlist configuration in the computers. Blue domain uses a group policy to deploy it.

* Blue computer can ping red computers with NETBIOS names.
* Blue server cannot ping red computers with NETBIOS names. But it works with full FQDN name.

<img width="600" alt="scheme" src="https://github.com/user-attachments/assets/19d2f8d1-38a1-4066-ad9e-e15cc6f28710">

When doing *ipconfig /all* on both computer and server, we can see the bug appearing.  
* Computers got both suffixes.  
* Server applied only one suffix.

When going on the network card configuration on the server, *both suffixes are installed*. Here is the issue !
## 2. The solution.

In order to solve the issue, you must deny the policy on Windows server. Then you will have to set manually the suffix searchlist in the network card.

## 3. Reproducing the issue

First we have to setup a Windows server 2012 R2 domain controler and add a group policy object that defines the DNS suffix searchlist:
![image](https://github.com/user-attachments/assets/da0b9030-8252-478c-a0de-2bde33756adc)

Then apply it on another Windows sever 2012 R2. You can check it's working by watching the network card settings:
![image](https://github.com/user-attachments/assets/2546f755-aabc-4c13-a2bd-a7e35e9a2727)

But when you try an *ipconfig /all*, only the first suffix will appear. Here is the issue:
![image](https://github.com/user-attachments/assets/f0116720-d937-41dc-9cdd-f0cab015471a)

