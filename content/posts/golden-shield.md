---
title: "Golden Shield: The Inner Workings of China's Great Firewall"
date: 2025-05-18T10:25:02-04:00
draft: false
---

## China Implements the Golden Shield

The Chinese government has recognized since 1998 that the internet had the power to expose their population to a world of information. They also knew that much of this information had the potential to challenge the official narrative of the communist party. Determined to get ahead of what they saw as an existential threat, the government began work on a massive censorship apparatus. This project was named the **Golden Shield** by its builders, but is known to the outside world as the **"Great Firewall of China."**  The first phase of the Golden Shield project wasn't completed until 2006, but it has continued to expand and is now arguably the most advanced system of its kind in the world.

The flow of information in China is regulated by both legislative and technical means. Both methods of control have the goal of upholding the social values of the **Communist Party of China (CPC)**. According to Go Click China:

> From the Chinese perspective, content filtration is entirely justified. Both flies and fresh air can come into the rooms by opening a window, figuratively speaking. As the Chinese economy started witnessing market reforms in the 1980s and opened up to international trade, the authorities felt the necessity to uphold its social values. This explains why the Ministry of Public Security of China decided to filter content that its residents would view over the internet. Therefore, the authorities drafted a plan and established their own set of principles. China’s ultimate goal is to make cyberspace safe for its people and the government.

It should be noted that the Great Firewall covers the entire region of mainland China, but does not cover **Special Administrative Regions** such as Hong Kong or Macau (at least not as of 2023).

## Most Foreign Websites are Blocked

From a technical aspect, information flow is managed by a sophisticated content filtering system. The list of foreign influences blocked by this system is extensive, including:

- Facebook
- X
- YouTube
- Google
- Wikipedia
- Quora
- News services such as ABC, BBC, and the Hong Kong Free Press
- LinkedIn
- Snapchat
- Spotify
- The Steam Store
- App Download Sites
- Many, many other platforms and services

The Chinese people have access to a tightly monitored group of alternative sites such as **WeChat, Tencent,** and **Sina Weibo**. These domestic services are tightly regulated by the government.

## How the Great Firewall Works

In a [previous post](https://merecat25.substack.com/p/censorship-and-surveillance-in-the?r=l5bk9), I discussed the Great Firewall in general terms. That post also defines many of the terms used here, so it is worth referencing. Here, though, I want to delve into how this massive censorship apparatus actually does what it does on a technical level. Although the Golden Shield is know colloquially as the "Great Firewall," it is actually a conglomeration of many different content management tools.  Recently, artificial intelligence technology has been integrated as well, which is discussed in the post [Artificially Muzzled: Digital Tyranny's New Face](https://merecat25.substack.com/p/artificially-muzzled-digital-tyrannys?r=l5bk9).

The CPC is very secretive about the actual capabilities and technical details of the Great Firewall. What information has been gleaned by outside experts has often relied on indirect methods such as **side-channel attacks**. According to Wikipedia:

> In computer security, a **side-channel attack** is any attack based on extra information that can be gathered because of the fundamental way a computer protocol, or algorithm is implemented, rather than flaws in the design of the protocol or algorithm itself (e.g. flaws found in a cryptanalysis of a cryptographic algorithm[](https://en.wikipedia.org/wiki/Cryptography "Cryptography")) or minor, but potentially devastating, mistakes or oversights in the implementation. (Cryptanalysis also includes searching for side-channel attacks.) Timing information, power consumption, electromagnetic leaks, and sound are examples of extra information which could be exploited to facilitate side-channel attacks.

What is known is that the flow of information into and out of China takes place via fiber-optic cables that travel through ten different monitored and controlled access points. Inside of China itself, content filtering methods include:

- IP Blocking
- DNS Poisoning
- URL Filtering
- TCP Reset Attacks
- Deep Packet Inspection (DPI)
- Fake SSL Root Certificates
- Active Probing

### IP Blocking

IP Address blocking is straightforward and inexpensive. It consists of having a blacklist of destination addresses that are blocks. Most likely, this is done by injecting routing information into the **Border Gateway Protocol (BGP)**, intercepting traffic to blocked websites. This is a process known as **Null Routing,** and is accomplished by monitoring the routers of the Chinese ISPs. Null routing causes all traffic to censored sites to be dropped. This form of IP address blocking is not resource intensive, so can be widely employed without taxing the system. Note, BGP is the protocol used to route information between different parts of the internet known as **Autonomous Systems (AS).**

### DNS Poisoning and Hijacking

DNS is how IP addresses are converted into names like google.com. The Great Firewall can **"poison" DNS** by causing the DNS server to return a falsified response. This is usually done in conjunction with IP blocking, and is somewhat more resource intensive. According to thousandeyes.com:

> Because changing domain names is not nearly as trivial as changing IP addresses, DNS-related techniques are often used in conjunction with IP blocking. DNS tampering involves falsifying the response returned by the DNS server, either through intentional configuration or DNS poisoning. The server can lie about the associated IP address, any CNAMEs related to the domain, the authoritative servers for the domain and the existence of the domain itself. As a result, users are given false responses for censored sites like Twitter, and websites are blocked at the domain level.

DNS Hijacking involves intercepting the DNS request and then "injecting" a different response. The user is directed to an alternative site (or to a null site). This technique ends up poisoning the DNS server too, because the server will cache the fake DNS response and return it when future requests are made.

### URL Filtering

Using **transparent proxies** and **server name indication (SNI)**, the Great Firewall can search for censored keywords and block access to URLs containing these words. Proxies sit between the user's computer and the destination server and scan URLs and HTTP requests for keywords. SNI allows multiple websites to be served by the same IP address and involves using proxies to send traffic to the right server during the TLS handshake. An unfortunate side effect to this technique of URL filtering is that blocking the IP address may also block access to uncensored sites as well.

### TCP Reset Attacks

A TCP reset attack involves packet injection. In this process, a **forged TCP packet** is injected into the communication stream. In the case of the Great Firewall, the injected packet causes the connection to terminate (an end-of connection request).

### Deep Packet Inspection

Deep packet inspection is a means of analyzing individual packets as they pass through a proxy or an **intrusion detection system (IDS)**. The Chinese system involves IDS-like technology. Legitimate IDS's scan packets as they pass through a firewall, looking for viruses, spam, and other unwanted content and filtering them out (the packets are blocked or rerouted).  The Chinese system, originally meant to check for illegal VPN use, has evolved. It now scans the packets, including the URL request, for blacklisted keywords. DPI employed by the Great Firewall is very advanced, making circumvention challenging. As of 2023, **ProtonVPN** reported that their VPN was unable to bypass it.

### Fake SSL Root Certificates

The Chinese government has created their own **Certificate Authorities (CA)**. These CAs will issue **Root SSL Certificates**. A legitimate Root SSL Certificate is the certificate used to sign all the other certificates issued by a CA. The Chinese government has control of all Root Certificates issued by CA on the mainland, and can use them to perform **Man-In-The-Middle (MITM) attacks**. Protonvpn.com offer a particular example of this technique:

> The most notable example occurred in 2015, when Google proved that the Chinese CA CNNIC was abusing its position of trust by issuing unauthorized digital certificates for several Google domains. In response, some browsers stopped accepting certificates issued by CNNIC. However, this block was not enforced on other Chinese CAs, and browsers continue to accept new Chinese CAs since.

### Active Probing

Active probing would most likely be used to detect circumvention tools. When someone, for example, tries to access a Tor bridge, the firewall can send a probe to see if the server is a Tor server. If it detects a Tor server or other censorship circumvention tool, the firewall blocks it.

## Circumventing the Great Firewall

&nbsp;Given all of the above technologies employed by the Great Firewall, it should be obvious that bypassing or circumventing this system is challenging. **Virtual Private Networks (VPN)** like ProtonVPN can't reliably circumvent it. **The Tor Network** would normally be the go to solution for bypassing censorship, but the Great Firewall can block it. So, are there any ways to circumvent the Great Firewall of China? Some of technologies employed can be bypassed, although not consistently. Sometimes Tor can be accessed via **bridges** or the **pluggable transport obfs4**. This tool makes Tor traffic look random. **Encrypted third-party DNS** services can counter DNS poisoning. **Encrypted Server Name Indication ( ESNI)** can be useful in circumventing URL filtering, because it makes it hard to detect the websites you connect to. [This article](https://www.wikihow.tech/Enable-ESNI) describes how to activate ESNI. According to the ProtonVPN blog this service is available for Firefox browser. Greatfire.org has a site called [Circumvention Central](https://cc.greatfire.org/en?Offer=abt_toc_def_ctrl) that has a graph of VPNs that shows how they perform against the Great Firewall. It rates both speed and stability of VPNs. As of this post, ExpressVPN and ProtonVPN perform the best. We've seen, though, that they are still frequently blocked.

Unfortunately it isn't going to get any easier to bypass the Firewall.  According to Radio Free Asia, police in China have increased spot searches of phones. They are looking for apps that are used to circumvent the government censorship apparatus. This has happened in Shanghai, Beijing, and Chengdu. According to the article, there is an **"anti-fraud" app** used by the police:

> A mobile phone repair specialist in the southern province of Guangdong who declined to be named for fear of reprisals said the police-approved “anti-fraud” app can also detect the presence of circumvention tools on any phone where it has been installed.
> 
> “As long as your phone has the anti-fraud app installed, they will know what you are doing,” she said.
> 
> “You have to be especially careful now if you want to get around the Wall.”

The Great Firewall of China is a sophisticated, draconian censorship behemoth consisting of many different technologies. These technologies work to make the Firewall seem nearly impenetrable. While this isn't far from the truth, there are some ways to bypass it, although the technology of the Firewall is constantly improving. The addition of artificial intelligence technology will add an even more chilling layer of state security. Circumvention methods will require constant tweaking to avoid being blocked by the "Golden Shield."

## Sources

https://protonvpn.com/blog/great-firewall-china?srsltid=AfmBOoq8nMbHVBnWMdEOxyzuVLkE0bSNjP3mxNSF6JQr84CVauqOqqoT

https://www.thousandeyes.com/blog/deconstructing-great-firewall-china

https://www.goclickchina.com/insights/the-complete-guide-to-the-great-firewall-of-china-gfoc/

https://cs.stanford.edu/people/eroberts/cs181/projects/2010-11/FreedomOfInformationChina/great-firewall-technical-perspective/index.html

https://www.techtarget.com/whatis/definition/Great-Firewall-of-China

[https://en.wikipedia.org/wiki/Side-channel\_attack](https://en.wikipedia.org/wiki/Side-channel_attack)

[https://en.wikipedia.org/wiki/Border\_Gateway\_Protocol](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)

[https://en.wikipedia.org/wiki/Server\_Name\_Indication](https://en.wikipedia.org/wiki/Server_Name_Indication)

https://support.torproject.org/glossary/obfs4/

[https://cc.greatfire.org/en?Offer=abt\\\_toc\\\_def\\\_ctrl](https://cc.greatfire.org/en?Offer=abt%5C_toc%5C_def%5C_ctrl)

https://www.rfa.org/english/news/china/phones-police-checks-03262024133232.html

