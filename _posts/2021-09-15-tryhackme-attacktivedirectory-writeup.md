---
title: Tryhackme Attackive Directory Writeup
author: Abhisar Pandey[MrGrep]
date: 2021-09-17 
categories: [Tryhackme, attacktivedirectory]
tags: [mimikatz ,bloodhound, active directory, kerberos, impacket, smbclient, winrm, hashcat]
math: true
mermaid: true
image:
  path: /assets/img/attackivedirectory.png
  width: 850
  height: 585
---

Hey, reader!! In this blog (writeup) I am going to discuss about the [Tryhackme](https://tryhackme.com) room [attacktivedirectory](https://tryhackme.com/room/attacktivedirectory).<br>
Before moving ahead I would like to strongly recommend you to first try this room on your own and then ,if you stuck anywhere, feel free to read this writeup :D<br>
so without any due let's get started.<br>
First thing you need to do is to click on `Start Machine` button, it will spin the machine for you.
> Note: While writing this writeup I had to start the machine thrice because of some issues because of that different IP will be shown.

<br>
## Task 1 & 2
Before moving ahead you need to download few things shown in the task-1 & 2 .  Now let's do it quickly with the simple script shown below<br>
```bash
sudo apt-get update -y
sudo apt-get update 
sudo git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket
pip3 install -r /opt/impacket/requirements.txt
cd /opt/impacket/ && python3 ./setup.py 
apt install bloodhound neo4j
```
## Task 3
After completing the Task 1 and 2 you need to start Enumeration with the NMAP<br>
```bash
┌──(mrgrep㉿kali)-[/opt]
└─$ sudo nmap -sSVC -A -T4 10.10.192.10                                  1 ⨯
[sudo] password for kali: 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-15 03:08 EDT
Nmap scan report for 10.10.192.10
Host is up (0.16s latency).
Not shown: 987 closed ports
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2021-09-15 07:08:57Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: THM-AD
|   NetBIOS_Domain_Name: THM-AD
|   NetBIOS_Computer_Name: ATTACKTIVEDIREC
|   DNS_Domain_Name: spookysec.local
|   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
|   Product_Version: 10.0.17763
|_  System_Time: 2021-09-17T11:02:27+00:00
| ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
| Not valid before: 2021-09-16T10:39:56
|_Not valid after:  2022-03-18T10:39:56
|_ssl-date: 2021-09-17T11:02:36+00:00; -1s from scanner time.
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).


``` 
As we can see clearly that we have 53,80,88,135,139,445,464,593,636,3268,3269 and 3389 port is open but we do not need to enumerate every single port.
Lets enumerate users using `enum4linux` tool.

![Desktop View](/assets/img/addr0.png){: width="972" height="589" }
_Enum4LINUX OUTPUT_

We can clearly see that known users are `administrator, guest, krbtgt, domain admins, root, bin, none`
Now moving to the questions of this task.

| Question                      | Answer          |
|:-----------------------------|:-----------------|
| What tool will allow us to enumerate port 139/445?| enum4linux    |
| What is the NetBIOS-Domain Name of the machine? | THM-AD   |
| What invalid TLD do people commonly use for their Active Directory Domain?| .local |

Before moving ahead, open your hosts file by typing `nano /etc/hosts` and add a line `<ip of tryhackme machine>     spookysec.local`

## Taks 4
Here we need to use [Kerbrute](https://github.com/ropnop/kerbrute/releases) tool as mentioned in the description to brute force and discovery of users, passwords and even password spray! So, make sure to clone that in your local computer. 
Lets git clone it but before that make sure you have golang installed.

```bash
sudo apt install golang-go
sudo git clone https://github.com/ropnop/kerbrute.git
cd kerbrute && sudo go build
cp kerbrute /usr/local/bin
```
Note: Several users have faced an issue that the latest version of Kerbrute does not contain the UserEnum flag in Kerbrute, if that is the case with the version you have selected, try a older version!

For this box, a modified [User List](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt) and [Password List](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/passwordlist.txt) will be used to cut down on time of enumeration of users and password hash cracking. It is NOT recommended to brute force credentials due to account lockout policies that we cannot enumerate on the domain controller.

![Desktop View](/assets/img/addr.png){: width="972" height="589" }
_to know about the flags used refer help menu_
<br>
Now moving to the questions of this task.

| Question                      | Answer          |
|:-----------------------------|:-----------------|
| What command within Kerbrute will allow us to enumerate valid usernames?| userenum    |
| What notable account is discovered? (These should jump out at you) | svc-admin   |
| What is the other notable account is discovered? (These should jump out at you)| backup |

## Task 5
As description suggests after the enumeration of user accounts is finished, we can attempt to abuse a feature within Kerberos with an attack method called ASREPRoasting. ASReproasting occurs when a user account has the privilege "Does not require Pre-Authentication" set. This means that the account does not need to provide valid identification before requesting a Kerberos Ticket on the specified user account.
For Retrieving Kerberos Tickets we can use a tool called "GetNPUsers.py" (located in impacket/examples/GetNPUsers.py) that will allow us to query ASReproastable accounts from the Key Distribution Center. The only thing that's necessary to query accounts is a valid set of usernames which we enumerated previously via Kerbrute.
![Desktop View](/assets/img/addr4.png){: width="972" height="589" }
_getting TGT_

Now save this in file called hash.txt and use hashcat over it.
```bash
hashcat -m 18200 -a 0 hash.txt passwordlist.txt --force
```

![Desktop View](/assets/img/addr_pss.png){: width="972" height="589" }
_password you can see red highlighted_

Now with this credential lets login in to the smbshare

Now Let's answer the questions

| Question                      | Answer          |
|:-----------------------------|:-----------------|
| We have two user accounts that we could potentially query a ticket from. Which user account can you query a ticket from with no password?| svc-admin   |
| Looking at the Hashcat Examples Wiki page, what type of Kerberos hash did we retrieve from the KDC? (Specify the full name) | Kerberos 5 AS-REP etype 23  |
|What mode is the hash?| 18200 | 
| Now crack the hash with the modified password list provided, what is the user accounts password? | find yourself :)



## Task 6
With a user's account credentials we now have significantly more access within the domain. We can now attempt to enumerate any shares that the domain controller may be giving out.

![Desktop View](/assets/img/addr2.png){: width="972" height="589" }
_here we can see that interesting share `backup`_

Now let's see what's in the share `backup`.


![Desktop View](/assets/img/addr1.png){: width="972" height="589" }
_here we have backup_credential.txt file_

Here we can see this backup_credentials.txt so get it in local and let's decode this 

![Desktop View](/assets/img/addr3.png){: width="972" height="589" }
_password you can see red highlighted_
We have got the creds for backup account. So first let's answer the questions of this task.

![Desktop View](/assets/img/addr5.png){: width="972" height="589" }
_Last two answers you need to get yourself as I don't want to ruin your experience_

## Task 7

As mentioned in the description itself, we have to use a tool within Impacket called "secretsdump.py". This will allow us to retrieve all of the password hashes that this user account (that is synced with the domain controller) has to offer and after exploiting this, we will effectively have full control over the AD Domain. we can use previously found password for backup account.
So lets use that :D

![Desktop View](/assets/img/addr6.png){: width="972" height="589" }
_for flag that is used you can check by `secretsdump.py -h`_

as we can see that we have got ntlm hashes of all the users now we can use pass the hash attack to login as that user without using password.

Now, lets quickly install `evil-winrm` by using command `sudo gem install evil-winrm` 

![Desktop View](/assets/img/addr7.png){: width="972" height="589" }
_we can see the flag to use hash_
Now before moving to next task lets see the answer of task 7

![Desktop View](/assets/img/addr8.png){: width="972" height="589" }
_we can see the flag to use hash_

## Task 8

Now as we have got the hash previously using secretsdump.py also we know how we can use evil-winrm to login so lets grab the flags :D

![Desktop View](/assets/img/addr9.png){: width="972" height="589" }
_We are logged in as admin_

So get the flag and submit it.
If you are reading this , I would like to thank you for your time. Have a great time ahead.
If you want to encourage me and support my work, you can support me via buy a coffee :)
<br>
<center><a href="https://www.buymeacoffee.com/0xMrGrep"><img src="https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=&slug=0xMrGrep&button_colour=ff0000&font_colour=ffffff&font_family=Lato&outline_colour=ffffff&coffee_colour=FFDD00"></a></center>





