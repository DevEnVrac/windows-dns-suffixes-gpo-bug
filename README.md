# This article is about a bug that impact Windows Servers OS that uses a GPO to define the DNS suffix searchlist.

## 1. Issue context and how to reproduce it.
This bug appeared while playing with domain relationship infrastructure, but can be reproced without any relationship.
It appeared when trying to ping NETBIOS name of computer of another domain name.  
*Got it with Windows Server 2012 and 2016. Not tested on newer versions.*

> On the following scheme: red domain uses manual suffix searchlist configuration in the computers. Blue domain uses a group policy to deploy it.

* Blue computer can ping red computers with NETBIOS names.
* Blue server cannot ping red computers with NETBIOS names. But it works with full FQDN name.

![GPO_suffix_bug drawio](https://github.com/NumNumV/windows-dns-suffixes-gpo-issues/assets/75941535/335096ff-a925-431a-afa8-05d24c26bc5d)

When doing *ipconfig /all* on both computer and server, we can see the bug appearing.  
* Computers got both suffixes.  
* Server applied only one suffix.

When going on the network card configuration on the server, *both suffixes are installed*. Here is the issue !
## 2. The solution.

In order to solve the issue, you must deny the policy on Windows server. Then you will have to set manually the suffix searchlist in the network card.
