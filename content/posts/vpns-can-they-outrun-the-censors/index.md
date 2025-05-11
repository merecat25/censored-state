---
title: "VPNs: Can They Outrun the Censors? What You Need to Know"
date: 2025-05-11T10:23:30-04:00
draft: false
---

## Virtual Private Networks

Virtual Private Networks (VPNs) are one of the most important tools in the fight against online censorship. A VPN can allow an internet user to surf the web with reasonable anonymity, access websites that may be blocked in their country, and sometimes even bypass advances censorship technologies. Because of these and other abilities, citizens of authoritarian regimes can get news, entertainment, and other information otherwise unavailable to them. A VPN, though, is not a panacea, and even the best VPNs have limitations.

Something to keep in mind is that censorship by repressive governments is not one-size-fits-all. Some countries use censorship technologies that are outdated and easy to bypass. This could be due to limited resources or to simple ignorance by authorities. So, for example, if the only technologies used are **IP address blocking**, **website blacklists**, or **DNS blocking,** bypassing censorship with a VPN might be trivial. Other countries, particularly China, use multiple types of sophisticated censorship tools and can block VPNs. **VPN use might also be a criminal offense** (China, Russia, North Korea, and Iran all have laws preventing or limiting VPN use). 

## **What a VPN *Can* Do**

A VPN creates an **encrypted tunnel** between your device and the VPN's server, and makes your internet traffic appear to come from a different geographical region. So, you could be in Russia, but your VPN makes your traffic appear to originate in a country such as Austria. This ability prevents internet service providers (ISPs) and governments from easily inspecting the information packets your computer is sending and receiving. VPNs also **mask your IP address**. IP addresses are how computers communicate, and can be used to identify where you are and possibly your identity as well. A good quality VPN may even help prevent **deep packet inspection (DPI)** used to monitor internet traffic in some countries. 

VPNs are not all created equal. And, VPNs use is easily detected and the VPNs themselves may be blocked. The ability to bypass sophisticated surveillance and censorship depends on how well a VPN **obfuscates traffic**. The hide.me blog identifies nine different obfuscation techniques that might be used by newer VPNs:

- **Randomizing Port Numbers**: Won't hide VPN use but may allow you to stay connected.
- **XOR Scrambling**: Makes data appear scrambled and works against simple DPI. Used by the OpenVPN protocol.
- **TLS-Crypt:** Adds a layer of encryption to packets, so each packet is encrypted twice. Can be very effective. Also used by OpenVPN.
- **Stunnel:** OpenVPN over SSL/TLS. Disguises VPN traffic as regular HTTPS traffic, and can hide the initial computer handshake.
- **Shadowsocks**: Your device connects to a SOCKS 5 proxy in another country. Also encrypts data to make it appear as HTTPS traffic. Used alone or in combination with a VPN protocol.
- **Obfsproxy**: "Wraps" traffic in encryption to disguise it as HTTPS traffic. Obsf4 makes traffic look like random noise and can be effective in bypassing DPI. Also works as a plug-in with the Tor Browser.
- **V2RAY VMess**: Encrypts traffic and routes it through a third-party server.
- **Domain Fronting**: Can separate the DNS/TLS request to a website from the HTTP connection. A request for a government-approved website could be routed to, say, the BBC.
- **Stealth Protocols**: Usually proprietary and you have to trust the VPN provider.

So, a VPN should be able to hide your location and identity, obfuscate internet traffic, and circumvent many censorship technologies.

## What a VPN *Can Not* Do

Now, that we've seen what a VPN can do to help circumvent censorship, let's look at some of the limitations of VPNs. To start with, VPNs provide good, but not complete, protection against censorship technologies. The OpenVPN protocol, for example, can be detected and blocked by **The Great Firewall of China**. One study used fingerprinting attacks and active probing to test the OpenVPN protocol and found weaknesses.  According to cyberinsider.com:

> The study’s findings should be a wake-up call for both commercial VPN providers and VPN users, especially those in regions with stringent censorship, such as when using a [VPN in China](https://cyberinsider.com/vpn/best/china/) with the “Great Firewall” blocking many users. It is clear that the current obfuscation mechanisms and fingerprinting countermeasures used in the industry could be upgraded, particularly if they are being consistently blocked in Regions such as China and Russia. That said, we still see some VPNs working in these regions and bypassing censorship efforts.

So, sometimes VPNs provide protection, but not consistently.

VPNs also don't provide absolute protection against **metadata collection**. All digital information contains metadata, and VPN providers may log metadata such as:

- **IP Addresses**
- **Data Packet Size**
- **Timing of Data Transfers and Connections**
- **Types of Files Transferred**
- **Employed Encryption Protocols**

Metadata does not directly identify you. If combined with other information, particularly combined with new AI-based surveillance methods, it can link you and your online activities. Metadata can reveal how you communicate, where you are, who you communicate with, and sites you have visited. VPN providers may **log metadata** in spite of "no-logging" claims.

Another known problem with VPNs is the leaking of user information due to configuration or protocol problems. **DNS leaks** occur when your VPN does not properly route DNS requests. The DNS request is instead sent to a default DNS server (such as the one at your ISP). This can happen without your knowledge. WebRTC is a peer-to-peer technology used for file sharing, video conferencing, and voice calls. **WebRTC leaks** occur when your IP address is revealed to third parties and websites. WebRTC is included by default in many browsers, including Chrome and Firefox. **IP address leaks** also occur. If a VPN fails to provide the most basic service of masking you r IP address, your identity and location are likely trivial to determine.

One thing a VPN definitely *can not* provide is **legal protection**. As noted above, many countries restrict or even criminalize VPN use. If you use a VPN in one of these countries, you could be subjected to fines or even prison. Also, since many VPN providers (particularly free VPN providers) log user data, they may have to release information in response to a warrant. No-log policies may sound good, but they only are effective if the provider actually honors them!

## Good VPN Practices

Given the pros and cons of VPNs discussed above, there are good practices that VPN users in repressive countries (and everywhere else, too) should employ:

Use a **paid VPN service**. A free VPN provider is making money somewhere, possibly by selling your data to third parties. I recommend ProtonVPN, NordVPN, and ExpressVPN.

Use **obfuscation techniques** discussed above, or use a VPN that provides them. Shadowsocks and Wireguard with TLS are good protocols.

**Location, location, location**. Use VPN providers located in countries with strict privacy laws (ProtonVPN is based in Switzerland, NordVPN in Panama).

**Test your VPN for leaks** using sites like [whatsmyipaddress.com](https://whatismyipaddress.com/) and [dnsleaktest.com.](https://www.dnsleaktest.com/) Repeat this periodically.

If you don't use a VPN that hides DNS requests, use a **secure DNS provider** like Quad9 DNS.

**Disable WebRTC** in your browser. Instructions for disabling this service in Chrome, Firefox, Safari, Edge, Opera, and Brave can be [found here](https://www.top10vpn.com/what-is-a-vpn/vpn-leaks/#how-to-fix-dns-leaks) (this site also discusses mitigation of many other leak types).

Use the **Tor Browser** or Tor-Over-VPN with pluggable transports such as obfs4, meek, or snowflake, especially if you are in a high-risk situation. Consider a Tor bridge if Tor is blocked.

**Don't use your real email** when signing up for a VPN service.

Use a VPN with a **kill switch** that prevents your IP address from being revealed if the connection is broken.

Consider **alternate operating systems** like Whonix, Kicksecure (with Tor), or Tails.

**Disable IPV6** in your settings, as this IP address protocol is easily identified.

Use **VPN alternatives** like Lantern. See the Circumvention Tools Page for more information

## Conclusion

As you can see, Virtual Private Networks can be excellent tools for censorship circumvention. They are not, however, designed to solve every problem. Using poorly configured VPNs or VPNs without additional tools to provide better obfuscation may expose information about you, up to and including your identity and location. Good practices when choosing and using VPNs can help mitigate some of the inherent weaknesses of VPNs.

## Sources

https://finevpn.org/how-can-you-disable-dpi-effective-tools-for-bypassing-deep-packet-inspection/

[https://en.wikipedia.org/wiki/Internet\\\_censorship\\\_circumvention](https://en.wikipedia.org/wiki/Internet%5C_censorship%5C_circumvention)

https://hide.me/en/blog/vpn-obfuscation-methods/

https://www.cactusvpn.com/beginners-guide-to-vpn/what-is-obfsproxy/

https://cyberinsider.com/openvpn-traffic-can-be-identified-and-blocked/

https://www.top10vpn.com/what-is-a-vpn/vpn-leaks/
