---
cover: \articles\article-3\in_depth_cover.webp
author:
  name: Konstantin Malevski
description: Coming Soon...
date: 2024-12-06T00:00:00.000Z
layout: article
---
# In-depth research publication: part 1
Research about steganography and stegomalware

### Table of Contents
1.  [Introduction](#introduction)
2.  [What is stegomalware?](#what-is-stego-malware)
3.  [Techniques of hiding data](#techniques-of-hiding-data)
4.  [Attack vectors](#attack-vectors)
5.  [First attempts at payload embedding](#first-attempts-at-payload-embedding)
6.  [How does AV combat stego?](#how-does-av-combat-stego) 

## Introduction
This article will be a part of a 2-chapter blog articles. 
This is chapter 1.

In this article you will learn about the current work I am doing
regarding stegomalware.

In this blog article I will share the most interesting finds from my experience with this project. 

if you want to read in detail about it click the link here: 🔗[*Research*](https://docs.google.com/document/d/1GQd9j6BegXcBhvopFVW1nb3cXH6Uz33FWW24O2g6I_Q/edit?tab=t.0#heading=h.jzcgav6ve78q)🔗

## Learning goals
- How data is hidden using steganography?
- What is stegomalware?
- What are the attack vectors of stegomalware?
- How does stego proves a challenge to modern AV solutions
- Windows AV processes and how it analyses malware
- How does modern AV combat such threats?
- Tools and frameworks used to bypass the AV


## What is stego-malware?
Stegomalware is a type of malware that uses steganography to hinder detection. 

Steganography is the practice of concealing a file, message, image, or video within another file, message, image, video or network traffic.

## Techniques of hiding data 
One of the most popular techniques utilized by adversaries to hide their malware is LSB Encoding.
![LSB](/articles/article-3/lsb_encodinf.png)

LSB encoding is one of the far more simpler techniques that adversaries use to hide data. The technique is executed by modifying the least significant bit of the file. 

By changing the LSB - the adversary keeps the overall metadata of the file untouched

## Attack vectors
We already concluded that stegomalware by using steganography is tough to deal with. Let’s explore what are the attack vectors which help us to recognize it:
![attack](/articles/article-3/attack.png)
#### File embedding
  - Stegomalware uses file embedding to hide the malicious payloads inside files such as images or audio. I covered LSB encoding which is used ro hide the payload but aiming to not change the overall size of the file. The bigger the image, the more useful that will be for the payload

#### Delayed execution
  - In order to stay hidden, adversaries make the payload to either execute with a big delay or execute when a specific event triggers on the victim’s system. Other malware types can also use such tactics but they rely on immediate execution and a lot of them get detected and stopped before being able to do serious damage
![delayed](/articles/article-3/malwarebaazar.webp)

#### Reverse_C2 connections
  - Alot of times stego-malware is used to hide payloads inside the images in order to try and dodge. I used CAPEV2 sandbox to analyse malware that has been used in images like pdf and png. I used MalwareBazaar for downloading the samples and running it in an isolated environment. 
![alt text](/articles/article-3/cape1.png)
  - The results from a lot of the analyses conducted are that most of the time types of malware when deployed try to interact with the system by spawning additional processes. The samples try to access files and modify registry keys which is a big red flag. That way the payload will most certainly try to disable a key security feature or try to elevate privileges
![cape2](/articles/article-3/cape2.png)
![cape4](/articles/article-3/cape4.png)

### Tools
There are various tools that enable us to embed messages or even paylaods inside images.
![Steghide](/articles/article-3/steghide_part1.PNG)

An example are OpenStego and Steghide which are fairly simple to use.
I played around with it for a while and decided to embed some payloads in images just to see what is the result.

## First attempts at payload embedding
During my research I got more and more interested in Windows AV so I decided that  I will make it my goal to bypass it, if not I will come out with alot of new knowledge about Windows Internals.

I selected Metasploit and Veil in combination to try and confuse the AV and not detect the payload, these were my first attempts in learning how the AV worked so it was not perfect. 

I wanted to simulate a scenario where a user visits a "bad" website, auto-downloads and executes the payload.

![veil](/articles/article-3/veil_part2.PNG)
![metasploit](/articles/article-3/metasploid_part2.PNG)
![apache](/articles/article-3/metasploid_part3PNG.PNG)

However just embedding the executable inside the image will not be enough, since Windows AV static code scanner will block the downloading of the payload.

Plus there is a big issue with the tools I used previously like Open-stego:
- they use the same mechanism for embedding and extracting the images which is something I m not fond of since I want the process of delivery to be automated.
![OpenStego](/articles/article-3/openstego1.PNG)

## How does AV combat stego?
Anti-virus solutions have traditionally safeguarded users against malware by scanning files against extensive databases of known threats. However, the emergence of stegomalware that conceals itself within seemingly innocuous files like images and PDFs—poses significant challenges for detection and prevention.

  - #### Signature detection
  One of the critical weaknesses in antivirus technology is its reliance on signature detection. When malware is embedded in an image without altering the image's signature, antivirus programs may fail to recognize it as a threat. Furthermore, many antivirus solutions lack advanced scanning algorithms to detect such threats.
![av2](/articles/article-3/av2.png)
  - #### Dynamic engine
  Modern antivirus solutions have begun integrating behavioral analysis and dynamic monitoring. These features create a controlled environment(sandboxing) where files can be executed safely. The antivirus then monitors the file's behavior for suspicious activities, such as making unauthorized API calls or attempting to gain elevated privileges
![av1](/articles/article-3/av.png)

### Why is this important?
The combination of dynamic analysis and behavioural analysis helps operators detect zero-day vulnerabilities which is a big neverlasting issue in cyber. 

Using sandboxing as well helps us determine and understand how the malware sample behaves inside the system - even if its a zero-day. 

![evolve](/articles/article-3/evolve.png) 
Even though people are currently working on improving AV detection using Machine learning, stego still is a big pain to detect and to eliminate. Malware is constantly evolving and can detect if its being run in a sandbox.

This will result of the malware to become dormant and wait until a specific event executes.

### What is next?
In this part I covered stegomalware, what attack vectors does it have, how it tries to bypass the AV and how the AV itself battles the problematic type of malware.

In the second chapter of the article we will look into tools and frameworks such as AMSITrigger, HavocC2, Sliver designed to bypass the Windows Defender. 

K@sio out!
![Logo](/articles/article-1/article1f.png)