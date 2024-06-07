---
cover: /articles/article-2/wos.png
date: 2024-06-06T14:23:00.000Z
description: Research on AD attacks.
layout: article
---
# Breaking the Vault: DC Sync and Golden Ticket attacks

**Playing around with Active Directory vulnerabilities.**
![CrackedWin](/articles/article-2/windows_cr.png)


👨‍💻 In this article we will take a dive into some of the interesting Active Directory attacks that have been conducted in the past on various environments.

If you like what I do,please feel free to follow me for more interesting articles 🔔 : 
    [Linkdin](https://www.linkedin.com/in/konstantin-malevski-b33837256/)

## Table of Contents
1. [What is DC Sync?](#What-is-DC-Sync?)
2. [What is Golden Ticket?](#What-is-Golden-Ticket?)
3. [Results](#Results)
4. [Recommendations](#Recommendations)


## What is a DC Sync attack?
A DC Sync attack is created in order to target domain controllers within Active Directory. 

This technique allows the attackers to impersonate a domain controller and request sensitive data, such as password hashes to gain access on other systems. 

#### What vulnerability does DC Sync exploit?
DC Sync attacks exploit a critical vulnerability in the way Active Directory handles replication requests. 

Specifically, this vulnerability lies in the permissions assigned to user accounts and how these permissions are managed within the domain.

![Replicatory_changes](/articles/article-2/ad_rep.png)

- Only domain controllers and high privileged accounts have rights to replicate data. If an attacker gains access to such account, they can mimic a domain controller.

- Using tools like Mimikatz, the attacker can request password hashes, as the system sees the attacker as a legitimate authority.

- The weakness comes from accounts having unnecessary replication rights.

### How does it work?
![DC Sync process](/articles/article-2/dc_sync.png)

- The attacker breaches the network and identifies the domain controller.
- Exploits misconfigured permissions to access account details.
- Uses tools like Impacket to request user hashes from the system.
- Employs Mimikatz to decrypt hashes, granting admin privileges in AD.
- The attacker gains unrestricted access to AD, allowing them to list and manipulate files.

![DC Sync image1](/articles/article-2/dc_image1.png)
![DC Sync image2](/articles/article-2/dc_image3.png)


## What is a Golden Ticket attack?
![ticket](/articles/article-2/ticket.png)

A Golden Ticket attack allows an attacker to generate a valid Kerberos Ticket Granting Ticket (TGT) and access any service within the AD environment. 

Discovered by Benjamin Delpy, the creator of Mimikatz, this attack exploits weaknesses in the Kerberos authentication protocol.
![mimikatz](/articles/article-2/mimikatz.png)
![mimikatz2](/articles/article-2/mimikatz2.png)

- The attacker first performs a DC Sync attack to obtain the krbtgt hash.
- Attacker then uses Mimikatz to forge a Golden Ticket using the krbtgt hash.
- With the Golden Ticket, the attacker can access the C$ drive and other resources without further authentication.

## Conclusion
By playing around with DC Sync and Golden Ticket attacks, we see the potential for significant damage if left unchecked. 

However once security personnel identified strong security measures, these attacks have become way less encontered on Active Directory.

![CrackedWin](/articles/article-2/windows_cr.png)



Stay tuned for more interesting news🔔

K@sio out.... 
![Logo](/articles/article-1/article1f.png)