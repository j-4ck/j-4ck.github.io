---
title: "Papercut NG RCE"
collection: publications
permalink: /publication/2023-06-19-paper-title-number-1
excerpt: 'This paper is about CVE-2023-27350'
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

In a statement released by Papercut, they claim the vulnerability is patched in versions released after 8 March 2023 (20.1.7+), with the immediate afvice to upgrade you Papercut application servers to one of the fixed "Maintenance Releases" listed on [their site](https://www.papercut.com/blog/news/rce-security-exploit-in-papercut-servers/)

How does the exploit work?
------

Many proof of concept exploits for this vulnerablity have since been released, including a neat metasploit rip and extensive documentation online - meaning we can really get into the nitty-gritty of what's going on here.
