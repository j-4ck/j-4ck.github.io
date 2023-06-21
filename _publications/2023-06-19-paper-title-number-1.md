---
title: "Papercut NG RCE"
collection: publications
permalink: /publication/2023-06-19-paper-title-number-1
excerpt: 'This paper is about CVE-2023-27350, AKA PapercutNG/MF RCE'
date: 2023-06-19
venue: 'Journal 1'
paperurl: ''
citation: 'Splunk https://www.splunk.com/en_us/blog/security/don-t-get-a-papercut-analyzing-cve-2023-27350.html, Papercut https://www.papercut.com/blog/news/rce-security-exploit-in-papercut-servers/'
---

This paper is about CVE-2023-27350, the new RCE exploit for print management service PaperCut NG
------

Attacks first leveraging CVE-2023-27350 are reported to have started on April 14 2023, though sources claim it is still being exploited up until present for threat actors trying to expand botnets and spread ransomware online. 
Of course, this is a huge security risk not only for members of the print management field but also anybody who may be using machines in the same environment as one of these systems; which could be unknown to you, at work and so on.

How dangerous really is this exploit?
------

With a nasty rating of "9.8 CRITICAL" from both NVD (National Vulnerability Database) and the ZDI (Zero-Day Initiative), one can only presume that the situation is serious. But why?
Affecting over 100 million users across the globe, an exploit of this capacity is never good news. The unauthenticated remote code execution is said to effect many versions, such as:
* PCMF/NG 8.0 or later
* PCMF/NG Application servers
* PCMF/NG Site servers

In a statement released by Papercut, they claim the vulnerability is patched in versions released after 8 March 2023 (20.1.7+), with the immediate advice to upgrade you Papercut application servers to one of the fixed "Maintenance Releases" listed on [their site](https://www.papercut.com/blog/news/rce-security-exploit-in-papercut-servers/)

How does the exploit work?
------

Many proof of concept exploits for this vulnerablity have since been released, including a neat metasploit rip and extensive documentation online - meaning we can really get into the nitty-gritty of what's going on here.
The attack takes place in a few stages, since a few variables have to be edited in order for it to work. In this publication I am going to be talking about the most popular POC for this exploit (provided by [Horizon3](https://github.com/horizon3ai/CVE-2023-27350)), though they are all very similar (with the MSF exploit using slightly different staging).

1. First, the attacker sends an HTTP GET request to the url "/app?service=page/SetupCompleted", in order to initiate the session and grab the necessary cookies.
1. Second, the attacker uses HTTP POST to /app to set variables "print-and-device.script.enabled" and "print.script.sandboxed" to "Y" and "N" respectively
1. The next stage is to navigate to the printer list page, so the attacker must send another HTTP GET request to "/app?service=page/PrinterList"
1. In this section the attacker must select a print device. This part of the process can yield mixed results, since if you try to get a hit on a printer ID that does not exist, you will not be able to acheive RCE. In the exploit proof provided by Horizon3, he points the script towards "/app?service=direct/1/PrinterList/selectPrinter&sp=l1001" using the default ID of "l1001", since this is most likely to land a hit.
1. This is really where the damage happens. After then HTTP GETting "/app?service=direct/1/PrinterDetails/printerOptionsTab.tab" in order to navigate to the printer options tab, the attacker updates the printer settings and uploads a malicious print job hook script, which triggers the arbitrary code execution.
1. Finally, the code executes and the exploit sets the initial variables back to their defaults ("N" and "Y")

I highly reccomend looking through the exploit code provided by Horizon3 [here](https://github.com/horizon3ai/CVE-2023-27350/blob/main/CVE-2023-27350.py)

What is the future for Papercut?
------

Well since Papercut have acknowleged the problem and swiftly released a patch, in theory the printer management reign of terror will soon come to an end as more and more people upgrade to newer / safer versions. Though, as is the situation with any majorly exploitable software, there will always be people using vulnerable versions until the end of time. And I predict this will just end up another exploit in the hand of people looking to expand botnets online by hitting any systems they can - and their will always be vulnerable systems. As for a sense of exploiting the individual - most enterprises and work stations should already be using the latest versions of Papercut NG/MF and should be out of the picture.



Thank you for reading my short publication on CVE-2023-27350. If you are looking for a more in-depth analysis, I strongly reccomend you check out [Splunk](https://www.splunk.com/en_us/blog/security/don-t-get-a-papercut-analyzing-cve-2023-27350.html)
