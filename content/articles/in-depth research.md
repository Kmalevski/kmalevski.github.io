---
cover: \articles\article-3\in_depth_cover.webp
author:
  name: Konstantin Malevski
description: Research about stegomalware
date: 2024-12-06T00:00:00.000Z
layout: article
---
# In-depth research publication: part 1
Research about steganography and stegomalware

### Table of Contents
1.  [Introduction](#introduction)
2.  [Learning goals](#learning-goals)
3.  [What is stegomalware?](#what-is-stego-malware)
4.  [Techniques of hiding data](#techniques-of-hiding-data)
5.  [Attack vectors](#attack-vectors)
6.  [First attempts at payload embedding](#first-attempts-at-payload-embedding)
7.  [How does AV combat stego?](#how-does-av-combat-stego) 
8.  [Why is this important?](#why-is-this-important)


## Introduction
This is the first of a two-part series where we explore stegomalware techniques, attack vectors, and antivirus challenges. 

The second part will focus on tools and advanced bypass strategies.


if you want to read in detail about it click the link here: 🔗[*Research*](https://docs.google.com/document/d/1GQd9j6BegXcBhvopFVW1nb3cXH6Uz33FWW24O2g6I_Q/edit?tab=t.0#heading=h.jzcgav6ve78q)🔗

## Learning goals
The goals of this article include:
- Understanding stegomalware and its significance.
- Exploring data-hiding techniques.
- Identifying stegomalware attack vectors.
- Learning how antivirus (AV) solutions address stegomalware.


## What is stego-malware?
Stegomalware is a type of malware that uses steganography to hinder detection. 

Steganography is the practice of concealing a file, message, image, or video within another file, message, image, video or network traffic.

## Techniques of hiding data 
One of the most popular techniques utilized by adversaries to hide their malware is LSB Encoding.
![LSB](/articles/article-3/lsb_encodinf.png)
> - Image showing how hackers use the least significant bit and modifying it to suit their payload.

LSB encoding is one of the far more simpler techniques that adversaries use to hide data. The technique is executed by modifying the least significant bit of the file. 

By changing the LSB - the adversary keeps the overall metadata of the file untouched

Other techniques include:
- Frequency Domain Steganography: Embedding data within frequency components of media.
- Masking: Concealing information in less noticeable regions of an image.

## Attack vectors
Stegomalware can be distributed using various methods:
![attack](/articles/article-3/attack.png)
#### File embedding
  - Stegomalware uses file embedding to hide the malicious payloads inside files such as images or audio. I covered LSB encoding which is used ro hide the payload but aiming to not change the overall size of the file. The bigger the image, the more useful that will be for the payload

#### Delayed execution
  - In order to stay hidden, adversaries make the payload to either execute with a big delay or execute when a specific event triggers on the victim’s system. Other malware types can also use such tactics but they rely on immediate execution and a lot of them get detected and stopped before being able to do serious damage
![delayed](/articles/article-3/malwarebaazar.webp)
> - Malware samples analyzed using MalwareBazaar in CAPEv2.
- Alot of times stego-malware is used to hide payloads inside the images in order to try and dodge. I used CAPEV2 sandbox to analyse malware that has been used in images like pdf and png. I used MalwareBazaar for downloading the samples and running it in an isolated environment. 
#### Command and control servers (C2)
Hackers use payloads in images to create a reverse TCP connection.

- A reverse shell connects to a C2 by having the payload, once executed on the target system, initiate an outbound connection (e.g., reverse TCP) to the attacker's C2 server, allowing the attacker to interact with the victim's terminal as if they were directly inside the target system.

![alt text](/articles/article-3/cape1.png)
> - the image shows analysis results on pdf malware samples using open-source sandboxing tool CAPEV2
  - The results from a lot of the analyses conducted are that most of the time types of malware when deployed try to interact with the system by spawning additional processes. The samples try to access files and modify registry keys which is a big red flag. That way the payload will most certainly try to disable a key security feature or try to elevate privileges
![cape2](/articles/article-3/cape2.png)
> - During the analysis Acrobat.exe was used to open the .pdf's inside the disposable machines and run the malware.

![cape4](/articles/article-3/cape4.png)
> - Alot of the malware samples when deployed immediately started interacting with the Windows API in order to create fake processes and extract information of the current OS

### Tools
There are various tools that enable us to embed messages or even paylaods inside images.
![Steghide](/articles/article-3/steghide_part1.PNG)
> - using the command line tool steghide to embed a secret message within a image

An example are OpenStego and Steghide which are fairly simple to use.
I played around with it for a while and decided to embed some payloads in images just to see what is the result.

## First attempts at payload embedding
During my research I got more and more interested in Windows AV so I decided that  I will make it my goal to bypass it, if not I will come out with alot of new knowledge about Windows Internals.

I selected Metasploit and Veil in combination to try and confuse the AV and not detect the payload, these were my first attempts in learning how the AV worked so it was not perfect. 

I wanted to simulate a scenario where a user visits a "bad" website, auto-downloads and executes the payload.

![veil](/articles/article-3/veil_part2.PNG)
> - creating an obfuscated payload using veil and metasploit

![metasploit](/articles/article-3/metasploid_part2.PNG)
> - using the created payload and starting the listener that waits for a connection.

![apache](/articles/article-3/metasploid_part3PNG.PNG)
> - Windows AV blocking the downloading of the file, thus this attempt is unsuccesfull.



However just embedding the executable inside the image will not be enough, since Windows AV static code scanner will block the downloading of the payload.

Plus there is a big issue with the tools I used previously like Open-stego:
- they use the same mechanism for embedding and extracting the images which is something I m not fond of since I want the process of delivery to be automated.
![OpenStego](/articles/article-3/openstego1.PNG)
> - OpenStego embedding payload within an image.

## How does AV combat stego?
Anti-virus solutions have traditionally safeguarded users against malware by scanning files against extensive databases of known threats. However, the emergence of stegomalware that conceals itself within seemingly innocuous files like images and PDFs—poses significant challenges for detection and prevention.

  - #### Signature detection
   Traditional AV relies on matching file signatures with known malware databases. However, stegomalware’s altered signatures often evade detection.
![av2](/articles/article-3/av2.png)
> - Antivirus checking a file’s signature against its database.
  - #### Dynamic engine
  Dynamic engines monitor file behavior in controlled environments, identifying suspicious activities such as unauthorized API calls or privilege escalations.
![av1](/articles/article-3/av.png)
> - Sandbox environment analyzing potential malware behavior.
### Why is this important?
The combination of dynamic analysis and behavioural analysis helps operators detect zero-day vulnerabilities which is a big neverlasting issue in cyber. 

Using sandboxing as well helps us determine and understand how the malware sample behaves inside the system - even if its a zero-day. 

![evolve](/articles/article-3/evolve.png) 
> - Modern malware adapting to evade detection.
Even though people are currently working on improving AV detection using Machine learning, stego still is a big pain to detect and to eliminate. Malware is constantly evolving and can detect if its being run in a sandbox.

This will result of the malware to become dormant and wait until a specific event executes.

### What is next?
This article covered stegomalware basics, attack vectors, and AV strategies. 

In Part 2, we will examine advanced tools like AMSITrigger, HavocC2, and Sliver for bypassing Windows Defender.

K@sio out!
![Logo](/articles/article-1/article1f.png)