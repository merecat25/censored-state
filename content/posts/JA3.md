---
title: "The JA3 Fingerprint as a Model of Censorship by Deep Packet Inspection"
date: 2025-06-29T08:59:17-04:00
draft: false
---

## JA3 Fingerprinting offers a good model for deep packet inspection by a government censorship apparatus.

![title_image](/images/ja3.jpg)

## Fingerprints of the TLS Handshake

When a client computer needs to establish a connection to a server, they need to "agree" on certain parameters like types of encryption used, identities, and session keys. **Transport Layer Security (TLS)** is used to encrypt the data transfer, and the **TLS Handshake** is how the two computers reach this agreement. Since the TLS handshake is not fully encrypted, it is possible to generate a **fingerprint** of this handshake. This information can be used for several legitimate reasons:

- Bot Detection
- Preventing Distributed Denial of Service Attacks
- Making Firewall Rules

Conceivably, though, these fingerprints could be used by a government censorship apparatus to identify profiles of **censorship circumvention tools** and methods. If they can be identified, then they can also be blocked.

## The JA3 Fingerprint 

One well-known TLS fingerprint is called the **JA3 Fingerprint**. This method of fingerprinting was developed by researchers at Salesforce in 2017. JA3 involves taking parts of the TLS Handshake, putting them together in a specific way, and making an MD5 hash that should be specific for a circumvention tool or protocol. According to keyinsight.com, the information extracted from the TLS handshake includes:

- TLS/SSL version
    
- List of supported cipher suites
    
- List of extensions
    
- Supported elliptic curves
    
- Elliptic curve point formats
    

## Suricata as a Deep Packet Inspection Tool

I felt like the JA3 fingerprint would be a good example to use to demonstrate how a censorship apparatus might detect the profiles of censorship circumvention tools. It's fairly straightforward to use **Suricata** to identify the JA3 hash. According to the infosecinstitute.com:

> Suricata is an open-source detection engine that can act as an intrusion detection system (IDS) and an intrusion prevention system (IPS). It was developed by the Open Information Security Foundation (OSIF) and is a free tool used by enterprises, small and large. The system uses a rule set and signature language to detect and prevent threats. Suricata can run on Windows, Mac, Unix and Linux.

It is free and open source, and it allows the user to add rules that generate alerts when it detects threats. Then, scripts can be written to trigger blocking using iptables rules (Linux firewall rules). For more information about Linux Host Firewalls, see my other blog [cyber-security-journey.com](https://cyber-security-journey.com/2021/09/06/linux-host-firewalls/).

*Note: I am not an expert in writing python code or bash scripting. Thankyou to ChatGPT for helping me create code, rules, and scripts for this lab writeup.*

## The Lab Setup

This lab setup involves using **Virtualbox virtual machines** set up so that one, Kali Linux, acts as a gateway to the internet, runs Suricata, and acts as a firewall (iptables). The other VM, Windows 10 or Ubuntu, acts as the "censored" computer. It has tools preloaded on it, including **Tor Browser**, **Psiphon**, and **Lantern** (loading the tools on a Windows 10 VM is easier, and Psiphon does not work on Ubuntu). A diagram of the setup looks like this:

<img src=":/c58562580f4d4ad5a43a5a9f79966b10" alt="setup2.png" width="496" height="405" class="jop-noMdConv">

For setting up Kali for forwarding traffic to the web as well as setting up Suricata, adding rules, and, generating logs see [Censored Lab](https://merecat25.substack.com/p/censored-lab).

## Attempting to Find the Tor JA3 Hash

Once the setup was done, the first thing I did was add rules to Suricata to detect the Tor TLS handshake (JA3 Hash) and also the TLS *ClientHello* message:

```bash
alert tls any any -> any any (msg:"Detected specific JA3 hash"; tls.ja3_hash; content:"<your_ja3_hash_here>"; sid:1003001; rev:1;)  
```

```bash
alert tls any any -> any any (msg:"Tor TLS JA3 Fingerprint"; ja3.hash; content:"e7d705a3286e19ea42f587b344ee6865"; sid:1000013; rev:1;)
```

Then I ran this script on Windows 10 after starting Suricata:

```bash
import requests

url = "https://www.cloudflare.com/"
headers = {
    "User-Agent": "JA3-Demo"
}

response = requests.get(url, headers=headers)
print(f"Status Code: {response.status_code}")
```

This generates a TLS handshake and causes Suricata to generate a log:

```bash
{
  "timestamp": "2025-06-28T09:42:24.584701-0400",
  "src_ip": "192.168.56.101",
  "dest_ip": "184.25.165.167",
  "sni": "www.microsoft.com",
  "ja3": "9225d95490794840d9d5f1f94d339285"
}
```

The JA3 hash is **9225d95490794840d9d5f1f94d339285**. As a side note, Suricata also shows the SNI (Server Name Indicator), so does a good job of profiling this connection.

Now, if I start the **Tor browser** on Windows 10 and browse to some websites, Suricata generates the following:

```bash
  "timestamp": "2025-06-23T12:56:07.741426-0400",
  "src_ip": "10.0.2.15",
  "dest_ip": "20.96.153.111",
  "sni": null,
  "ja3": null
}
{
  "timestamp": "2025-06-23T12:56:07.741409-0400",
  "src_ip": "192.168.56.101",
  "dest_ip": "20.96.153.111",
  "sni": null,
  "ja3": null
```

As you can see, the SNI and JA3 are "null." What is happening is that Tor is likely using **pluggable transports** to encrypt or randomize the TLS handshake (or not use TLS at all), preventing the JA3 fingerprint from being calculated. Of course, the assumption is that if JA3 can't be detected, then making a rule to block it would be impossible.

## Attempting to Find the Psiphon JA3 Hash

Besides Tor, there is another tool commonly recommended for censorship circumvention. According to ifex.org:

> Psiphon is a tool for computers running Microsoft Windows and Android phones, designed specifically to evade Internet censorship. Unlike tools such as Tor, Psiphon employs a variety of technical tricks rather than one specific design to help users evade online censorship.

**Psiphon** routes traffic through its own network and uses "**multiple transport systems**." If one transport protocol is blocked, Psiphon can find a another fast connection quickly. It's primary use is to access sites that are blocked by censors. This makes it different than Tor, which is used to truly "anonymize" traffic.

For this test, I added a rule to Suricata for Psiphon:

```
alert tls any any -> any any (msg:"Possible Psiphon TLS JA3 detected"; ja3.hash; content:"b8419f4529b976b01d86fb910efac57f"; sid:1003001; rev:1;)
```

Running Psiphon on Windows generates the following log:

```bash
{
  "timestamp": "2025-06-28T10:27:34.055059-0400",
  "flow_id": 1672448868015442,
  "in_iface": "eth1",
  "event_type": "tls",
  "src_ip": "192.168.56.101",
  "src_port": 50638,
  "dest_ip": "104.18.165.46",
  "dest_port": 443,
  "proto": "TCP",
  "pkt_src": "wire/pcap",
  "tls": {
    "subject": "CN=psiphon3.net",
    "issuerdn": "C=US, O=Google Trust Services, CN=WE1",
    "serial": "0C:89:D3:E9:77:7A:AE:8B:0D:C9:B4:26:A8:D3:69:71",
    "fingerprint": "ac:2e:96:93:59:f9:49:8c:7e:a8:5a:22:69:aa:fa:25:f9:92:d3:84",
    "version": "TLS 1.2",
    "notbefore": "2025-05-26T05:21:32",
    "notafter": "2025-08-24T06:21:29",
    "ja3": {
      "hash": "9bc3bac1b476436b9fd2afa48def4836",
      "string": "771,4866-4865-4867-49200-49191-49196-156-49187-49199-60-49162-49172-53-47-10-49161,11-35-13-45-5-65281-43-18-10-51,29-23-24,0"                                                                                         
    },
    "ja3s": {
      "hash": "c31c6020fe8bdc6d29ef54025152b64
```

So, in the case of Psiphon, Suricata is able to generate the JA3 hash from the TLS handshake. Suricata also identifies Psiphon as the tool. So, in this case, a rule could be made to block Psiphon. So, at least for a basic **Deep Packet Inspection** **(DPI)** tool, Tor would seem to a better tool for censorship avoidance.

## The Real World Looks a Little Different

In real life, things are not this simple. Tor may be able to obfuscate the JA3 hash, but DPI used by censors can look for much more than just JA3. A partial list of other parameters DPI may look for could include:

- TLS Version
- Cipher Suites
- User-Agent
- Domain Name
- DNS over HTTPS
- Ports
- Destination IP Addresses
- Multiple Others

Also, Tor might be identified and blocked by censors using traffic analysis patters, IP blocking (of bridges and relays), and other methods. Psiphon can be blocked using similar techniques. Censorship tools like Tor and Psiphon are engaged in a constant "cat-and-mouse" game with censors. According to thousandeyes.com:

> Common anti-censorship tools used to fly under the radar include Tor, proxy servers, VPN services and even code words. However, the Great Firewall has continued to innovate to detect and block these circumvention methods, and as a result the two sides have evolved in tandem in what is essentially a digital war over access to information.

Hopefully, this exercise using the JA3 fingerprint as a model, has demonstrated how censors, using deep packet inspection tools, could detect fingerprints of censorship circumvention tools, and how some tools circumvent these detection methods.

## Sources

https://github.com/OISF/suricata

https://www.fastly.com/blog/the-state-of-tls-fingerprinting-whats-working-what-isnt-and-whats-next

https://www.keysight.com/blogs/en/tech/nwvs/2024/06/26/ja3-randomization

https://www.infosecinstitute.com/resources/network-security-101/suricata-what-is-it-and-how-can-we-use-it/

https://www.thousandeyes.com/blog/the-war-between-chinas-great-firewall-and-circumvention-tools

https://ifex.org/fighting-censorship-with-circumvention-tools-tor-psiphon/

https://www.thousandeyes.com/blog/the-war-between-chinas-great-firewall-and-circumvention-tools