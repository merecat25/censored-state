---
title: "Censored Lab"
date: 2025-05-31T13:28:03-04:00
draft: false
---
Welcome to Censored Lab!

![lab pic](/images/lab_picture.jpg)

### General Information

For general information about how censorship works, and information about IP Addresses, DNS, Deep Packet Inspection, and other subjects, see:

[Censorship and Surveillance in the Digital Age: Part One](https://merecat25.substack.com/p/censorship-and-surveillance-in-the)

[Censorship and Surveillance in the Digital Age: Part Two](https://merecat25.substack.com/p/censorship-and-surveillance-in-the-5a4)

[Golden Shield: The Inner Workings of China's Great Firewall](https://merecat25.substack.com/p/golden-shield-the-inner-workings)

[Behind the Blackout: The Mechanisms, Monitoring, and Impact of Internet Shutdowns](https://merecat25.substack.com/p/golden-shield-the-inner-workings)

See also the [Resources Page](https://merecat25.substack.com/p/circumvention-tools) of this blog for links to anti-censorship tools and other informational sites.

### Lab Setup

For simulating online censorship, a lab can be set up using Virtualbox installed on a host computer. Virtualbox can be downloaded for **Windows, MacOS**, and **Linux** from [HTTPS://www.virtualbox.org](https://www.virtualbox.org/). Be sure to download and install the **extension pack** as well. For Ubuntu and other Linux distros, be sure to download the correct edition (i.e. Ubuntu 24.02) because unmet dependencies can be a problem. I recommend four different virtual machines:

- **IPFire** as a firewall. Will be used for simulating the actions of a government censor. [Download here](https://www.ipfire.org/downloads/ipfire-2.29-core194).
    
- **Windows 10** as one of the censored computers. Should have Psiphon, Lantern, Tor, and a VPN installed. Use the [Microsoft Media Creation Tool](https://www.microsoft.com/en-us/software-download/windows10).
    
- **Kali Linux** for running Wireshark. Can also be used as a censorship platform with iptables and dnsmasq (discussed later). [Download here](https://www.kali.org/).
    
- **Ubuntu 24.04** as another censored machine. [Download Here](https://ubuntu.com/download).
    

*Note: I have had different firewalls work better with certain hosts. IPFire works well on Windows, but other choices include OPNSense (complicated), PFSense (a little less complicated), ClearOS (hard to install) among others. IPFire is easy to install and configure.*

The initial setup will look like this:

![Network Diagram](/images/setup1.png)

The IPFire virtual machine (VM) acts as our firewall, hands out IP Addresses to the other VMs via DHCP, and acts as a DNS server. When IPFire is set up on Virtualbox, there is a GREEN interface and a RED interface. In our case, the green interface is set to the internal network and the red interface to NAT which provides internet access via the host computer. Instructions for installing IPFire on Virtualbox can be found [HERE](https://www.ipfire.org/docs/installation/virtual-box), and instructions for IPFire networking [HERE](https://www.ipfire.org/docs/installation/step5).

The subnet for the internal network is 192.168.1.0/24, and the IPFire VM has an IP Address of 192.168.1.1. Windows 10, Kali, and Ubuntu are all set to internal networking only. This means they can only access the internet via the firewall. So, network settings for all three need the following settings:

- Receive IP Address automatically (DHCP from IPFire)
    
- DNS server to IPFire IP Address (192.168.1.1)
    
- I disabled IPV6 for simplicity, but in real life IPV6 would also have to be blocked to censor access to DNA, etc.
    

For Kali, install Wireshark as follows (if not already installed);

```bash
sudo apt update
sudo apt install wireshark
```

### DNS Blocking and Poisoning

Blocking DNS can be challenging. Modern browsers will fallback to any available DNS servers unless they are all blocked. Blocking DNS (port 53) only works if the browsers don’t have HTTPS over DNS (Port 443). Blocking all traffic to 443 blocks all websites, not just censored sites. Windows caches DNS, so the cache has to be cleared for previously visited sites.

With port 53 and port 443 NOT blocked and DNS servers not blocked, an nslookup (Windows) or dig (Linux) for bbc.com is run and works. Accessing BBC on firefox while Wireshark is running shows the DNS requests:

![dns open](/images/dns_open.png)

Blocking all common DNS servers but not blocking all traffic to port 443 on IPFire and doing the same thing shows a different result. This involved adding rules blocking port 443 for

- **Cloudflare** 1.1.1.1, 1.0.0.1 2606:4700:4700::1111 (IPv6)
    
- **Google** 8.8.8.8, 8.8.4.4 2001:4860:4860::8888 (IPv6)
    
- **Quad9** 9.9.9.9, 149.112.112.112
    
- **NextDNS** 45.90.28.0/24, 45.90.30.0/24
    
- **OpenDNS** 208.67.222.222, 208.67.220.220
    
- **CleanBrowsing** 185.228.168.168, 185.228.169.168
    
- **AdGuard** 94.140.14.14, 94.140.15.15
    
- **Comodo Secure DNS** 8.26.56.26, 8.20.247.20
    
- **Neustar UltraDNS** 156.154.70.1, 156.154.71.1
    
- **Cloudflare for Families** 1.1.1.2, 1.0.0.2
    

When this is done, *ping 8.8.8.8* works because it doesn’t require DNS, but *dig google.com* does not work (DNS is blocked). Lantern also does NOT work with DNS blocked. Tor does not access BBC.com but will connect. Wireshark with filters for port 53 or 443 doesn’t show any captures. This mimics real-world censorship in a country like China.

So, how does a person surfing the web in China get to a website since all foreign DNS serves are blocked? China has servers run by the government. This gives the PRC tight control over what sites can be reached (combined with other methods of censorship). The state controlled DNS servers can be configured to allow access to approved sites or to block or poison requests for censored sites.

**DNS poisoning** can be demonstrated using dnsmasq on Kali. This requires a reconfiguration of the Virtualbox network so traffic from Windows or Ubuntu goes through Kali instead of IPFire. The new setup will look like this:

![setup two](/images/setup2.png)

Kali has dnsmasq installed and also has the Linux firewall iptables. The dnsmasq program will be used to mimic DNS poisoning and can be installed with the commands:

```bash
sudo apt update
sudo apt install dnsmasq
```

**DNS on Ubuntu is set to Kali’s IP**. To setup dnsmasq on Kali to block or poison DNS to certain sites, the config file needs to be edited:

```bash
sudo nano /etc/dnsmasq.conf
```

Then edit the file:

```bash
# Basic DNS settings
port=53
domain-needed
bogus-priv
no-resolv

# Use public DNS servers upstream
server=1.1.1.1
server=8.8.8.8

# Spoofed/poisoned domains
address=/facebook.com/0.0.0.0
address=/youtube.com/0.0.0.0
address=/bbc.com/0.0.0.0
address=/twitter.com/0.0.0.0
address=/instagram.com/0.0.0.0
address=/wikipedia.org/0.0.0.0
```


Save this then exit. Now when Ubuntu is forced to use Kali’s DNS, *dig facebook.com* does not work, but *dig google.com* does. The DNS is poisoned and redirects to 0.0.0.0. This shows how DNS could be blocked by cenosrs. The problem with this method is that the censored computer (Ubuntu) has to have to many configuration changes to make it work. (I had to edit two other files to really block DNS). With Windows 10 instead of Ubuntu, though, DNS was blocked with just dnsmasq changes (the browser could not reach any blocked sites, but could reach any other site).

If Wireshark is run for the requests and filtered for DNS and DNS over HTTPS, surfing to google.com shows the normal DNS request:

![google dns](/images/censored-lab/google_dns.png)

bbc.com on the other hand, shows:

![bbc dns](/images/censored-lab/bbc.png)

What’s really interesting, this was done with DNS over HTTPS off in Firefox. If Privacy and Security settings are changed to use DNS over HTTPS instead, the sites can be accessed. To block this too, DNS over port 443 for all or most common DNS servers has to be blocked as above.

Another interesting thing we can do is set up dnsmasq on Kali to log all attempts to reach blocked sites. We have to modify the config dnsmasq config file again and add

```bash
log-queries
log-facility=/var/log/dnsmasq.log
```

the restart dnsmasq:

```bash
sudo systemctl restart dnsmasq
```

and start logging:

```bash
sudo tail -f /var/log/dnsmasq.log
```

If the user on Windows tries to browse to facebook.com, the censor will see

![log](/images/bash.png)

## Conclusion of This Section

The goal of all of this is to simulate how a repressive government like China could block and poison DNS. One thing to add: since I am not an expert in networking (although I have a Network+ certification), **I LIBERALLY relied on ChatGPT and Perplexity to help resolve problems and develop testing ideas**. All the writing is mine, but much of the technical stuff would have been difficult without AI help.
