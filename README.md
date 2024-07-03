# This article is about a bug that impact Windows Servers OS that uses a GPO to define the DNS suffix searchlist.

## 1. Issue context and how to reproduce it.
This bug appeared while playing with domain relationship infrastructure, but can be reproced without any relationship.
It appeared when trying to ping NETBIOS name of computer of another domain name.

> On the following scheme: red domain uses manual suffix searchlist configuration in the computers. Blue domain uses a group policy to deploy it.
## 2. The solution.
