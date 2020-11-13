# Create a Jenkins CI CD pipeline to deploy project website on AWS
## Problem statement:
Need to implement CI CD pipeline on jenkins as per below instructions –
* Create a jenkins master node and two slave nodes (slave 1 Test server) and (slave2 Production server) at AWS.
* Configure jenkins thus it Install the project website from git at slave-1 testing server on docker first, if successful then it should built the project website on slave2-production server. 
* Create pipeline view and Trigger the job using git web-hooks , whenever any change at git repository then jenkins should notify it and jenkins will automatically start build process.

## Prescription:
1.	Create Jenkins Master and slave infrastructure at AWS 
2.	Check git repository
3.	Install docker at slave nodes
4.	Configure build job for Testing server
5.	Configure build job for Production server
6.	Configure job; after Test server job done success it will build job for Production server 
7.	Create Pipeline view to run the job
8.	Create Web hook to initiate job when commit has made at GIT repository

## Implementation:
1.	Create Jenkins Master and slave infrastructure at AWS
create Jenkins master and slave architecture in AWS like below in AWS . here one instance will be Jenkins master and other two instances will be jenkins slave nodes namely Testing server and Production server 

![title](./picture/picture1.png)

** To create the infrastrucre perform below steps:

* Launch three instance at AWS with Ubuntu:18.04 AMI , note that all three nodes need to be in same aviability zone, otherwise AWS can charge for bandwidth for different aviability zone.

![title](./picture/picture2.png)

** Allow port 22, 80, ICMP, 8080 in security group 

** Create ssh key at Jenkins master node and copy them to slaves nodes authorized_keys section and login from jenkins master node to all slave nodes once
```
ubuntu@ip-172-31-47-27:~$ cd ~ 
ubuntu@ip-172-31-47-27:cd .ssh/
ubuntu@ip-172-31-47-27:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:JghMm5W3533KM4isNY3KS/UHnAx4dxa3+tmlLcNaxFU ubuntu@ip-172-31-47-27
The key's randomart image is:
+---[RSA 2048]----+
|  . ..    . .   E|
| o +...    o .  .|
|  = ..o.. o .   .|
|   . o.=.+ . . . |
|    . ooS..   o .|
|     . *.....= + |
|    ..+.oo.oo B .|
|   o oo...=  o o |
|    =o     o.    |
+----[SHA256]-----+

cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2INJ3xTJnUepfMyLnqQB6IojESx+44/QKWbLpeWJsJUHaLh6k9nscZVt8OD4XA/cTPCrhhrcciC0p9PHK4xF+HHDavvesQSjTlxzevv5GLFTbGNsyFLvHunpFA1Zwh0YbaASJB9VhGkasHwa2uQ2iPDvC5lv20cmsWVXrL9+ODDNpDTrsGv+ntGzjcD1ETiRjvDXALrUy2c0g8mJQIa92Ie3nQTUtbKZiDsusEE2Px/D2GazgQiuLQ6n3q4Wyp/WrLJsLV2FVj4I4ZClDCQWq4UXymKDXebUhIo3jbP5+/hjC/PDo2mqWS8E4u9fwWofJokIOWFZfnDfOrxdmgPfF ubuntu@ip-172-31-47-27

```

Now the newly created ssh keys need to install at both slaves nodes at authorized_keys section.login at both slaves nodes and paste it into authorized_key section :
```
cd ~/.ssh/
echo AAAAB3NzaC1yc2EAAAADAQABAAABAQC2INJ3xTJnUepfMyLnqQB6IojESx+44/QKWbLpeWJsJUHaLh6k9nscZVt8OD4XA/cTPCrhhrcciC0p9PHK4xF+HHDavvesQSjTlxzevv5GLFTbGNsyFLvHunpFA1Zwh0YbaASJB9VhGkasHwa2uQ2iPDvC5lv20cmsWVXrL9+ODDNpDTrsGv+ntGzjcD1ETiRjvDXALrUy2c0g8mJQIa92Ie3nQTUtbKZiDsusEE2Px/D2GazgQiuLQ6n3q4Wyp/WrLJsLV2FVj4I4ZClDCQWq4UXymKDXebUhIo3jbP5+/hjC/PDo2mqWS8E4u9fwWofJokIOWFZfnDfOrxdmgPfF ubuntu@ip-172-31-47-27  >> .ssh/authorized_keys
```

