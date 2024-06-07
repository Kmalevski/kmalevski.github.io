---
cover: /articles/article-1/malware-cover.jpg
author:
  name: Konstantin Malevski
date: 2024-05-22T00:00:00.000Z
layout: article
---

# Building a payload validation lab
My 6-month internship at Outflank

Check out their website here and reserve a meeting [*Outflank:*](https://www.outflank.nl/)
![Outflank logo](/articles/article-1/outflank1.png)

### Table of Contents
1.  [Introduction](#introduction)
2.  [Installation and configuration](#installation-and-configuration)
3.  [Integration of tools with Python](#integration-of-tools-using-python)
    - [PeSieve](#pesieve)
    - [Moneta](#moneta)
4. [Parsing the result's JSON file](#parsing-the-results-json-file)
5. [Challenges](#challenges)
6. [CAPEv2 source code issues](#capev2-source-code-troubles)
7. [Results](#results)


## Introduction
Hello, cybersecurity enthusiasts! I'm Konstantin and I'm excited to share my six-month internship journey at Outflank.  This experience was centered around developing a sophisticated payload validation lab using open-source sandboxing solutions.

The main objective of my internship was to build an isolated lab capable of systematically validating malware payloads. 
 This lab would ensure that payloads behave as expected and identify indicators of compromise (IOCs) 


## Research
The initial phase of the project involved extensive research on various sandboxing solutions. After comparing Cuckoo Sandbox, Drakvuf, and CapeV2, we selected CapeV2 for its superior automation, detailed reporting, and integration capabilities. 

You can check their respective github repositories below:

 [*Cuckoo Sandbox*:](https://github.com/cuckoosandbox/cuckoo)
![Cuckoo](/articles/article-1/cuckoo.png)

[*Drakvuf*:](https://github.com/tklengyel/drakvuf)
![Drakvuf](/articles/article-1/drakvuf.png)

[*CAPEv2*:](https://github.com/kevoreilly/CAPEv2)
![CapeV2 repository](/articles/article-1/CAPEv2.png)

## Installation and configuration
Setting up CapeV2 required configuring a secure environment using isolated virtual machines. 

This setup ensured that the analysis of potentially harmful payloads would not affect the host system. 

The process involved installing dependencies, configuring network settings, and setting up Windows analysis machines.

![Architecture CapeV2](/articles/article-1/capearchitecture.png)
> CapeV2 architecture shows the interaction between host and guest machines

I successfully installed and configured CapeV2 in a Linux environment. This involved addressing various technical challenges and troubleshooting issues to ensure a stable setup. 
The sandboxing solution is known for its complex setup and it delivered on that part XD.

> Issue during running the installation script for all dependencies
![Architecture CapeV2](/articles/article-1/issue1.png)


> Virtualization module not enabled in Ubuntu resulting in freezing the virtual machine
![Architecture CapeV2](/articles/article-1/issue2.png)

> Dashboard of sandbox
![Dashboard cape](/articles/article-1/dashboard.png)

 
## Integration of tools using Python
Tools implemented:
- ##### [PeSieve](https://github.com/hasherezade/pe-sieve)

- ##### [*Moneta*](https://github.com/forrest-orr/moneta)

A significant part of my internship was integrating tools like PE-Sieve and Moneta into CapeV2. PE-Sieve and Moneta help detecting in-memory modifications, which is essential for the payload validation process. I added them for an extra layer of scanning.

I used Python to write scripts that ran the tools as an auxiliary module and became part of CAPEv2's system.
That way I can retrieve the results from the tools that run on the PE file.

![pesieve_output](/articles/article-1/pesieveOutput.png)

![moneta_output](/articles/article-1/moneta.png)

![retrieved_files](/articles/article-1/results.png)


## Parsing the result's JSON file
I developed a Flask application to parse and display data from PE-Sieve's JSON reports. This application transformed raw data into a more readable format, making it easier for analysts and operators to interpret the results.

![JSON parser](/articles/article-1/parsers.png)

![json_parse2](/articles/article-1/json_parse2.png)

## Challenges
hroughout the internship, I faced and overcame numerous challenges, from dependency issues during installation to debugging code integrations. Here are some of the key challenges and how I resolved them:

-  During the initial setup, I encountered several dependency conflicts. CAPEv2 is very dependency sensitive which is a problem beacuse even if 1 dependency conflicts with others, the sandboxing platform will become unstable and unusable. 

![issue3](/articles/article-1/issue3.png)

- Integrating tools like PE-Sieve and Moneta required significant knowledge of CAPEv2's functionalities as well as Python programming skills coding. 
![issue4](/articles/article-1/issue4.png)

### CAPEv2 source code troubles
Part of the source code of CAPEv2 did not work properly which made the integration of tools more difficult.It is caused by deprecated code from the past because CAPEv2 comes from Cuckoo sandbox.  This issue affected a functionality within the CapeV2 sandbox.


This function is essential because it passes the Process Identifier (PID) of the payload sample to all auxiliary modules, allowing them to execute on the specified process

![source_code3](/articles/article-1/source_code_issue3.png)

**How I proceeded to eliminate this problem was:**
- alot of log statements were added to check where my code breaks
- made sure add_pid_to_aux function was explicitly called at the start of the analysis
- Alot of test and failure processes were made to continiously test the changes I made

![source_code1](/articles/article-1/issue5.png)

After a while I managed to fix the issue.

The solution to the problem was simple at first sight but alot hours of troubleshooting sit behind it. The loop of fetching the PID's to the aux modules was not working, so I put it in another loop which did what I wanted. The function was working as expected.
![source_code1](/articles/article-1/source_code_issue4.png)

## Reflection
My six-month journey at Outflank was both challenging and rewarding. From going through the complex setup of CapeV2 to integrating advanced tools for malware analysis, every step was a learning curve. I hope to continue exploring and contributing to the field of cybersecurity. 


## Results
- Research on available sandboxing solutions
    > Extensively researched various sandboxing solutions to identify the one best suited for the project. 
- Provided guide on installation and configuration of CAPEv2
    > Installed and configured CapeV2 in a Linux environment, addressing various technical challenges and troubleshooting issues.
- Integration of PeSieve and Moneta tools for checking modified PE files
    > I have successfully added those 2 tools for scanning memory modifications in CAPEv2
- Hypothesis testing
    > Implemented a method/process for validating specific hypotheses about malware behavior using the integrated tools.



### Additional resources
If you want to read more about my research and findings click the links below:

[*Research*](https://docs.google.com/document/d/1D4KcmOjHAVkuApVuXmvaG8e39ci4o1W2-LJkrHW9Kbg/edit?usp=sharing)

[*Code development*](https://docs.google.com/document/d/16ljtHHjs8Ykw86qGVeSWeJnc0pqLZHA10ZUEeeE5b6w/edit?usp=sharing)



Stay tuned for more news🔔

K@sio out.... 
![Logo](/articles/article-1/article1f.png)