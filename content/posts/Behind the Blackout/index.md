---
title: "Behind the Blackout: The Mechanisms, Monitoring, and Impact of Internet Shutdowns"
date: 2025-05-31T11:47:03-04:00
draft: false
---
### Internet shutdowns may be purposeful or due to poor infrastructure, but their occurrence and impacts can be measured.

![title picture](/images/title.jpg)

## Internet Shutdowns: Exams, Protests, and Infrastructure Failures

On two days this week, May 20th and May 22nd, monitors of internet connectivity showed **widespread internet outages in Iraq.** This occurred on both days from 03:00 and and 05:00 UTC (6:00 AM to 8:00 AM) and coincided with national school exams in the country. The move, ordered by the Iraqi government, is designed to prevent cheating during the exams. Since 2022, internet shutdowns during exams have occurred over 100 times. Digital rights groups such as Access Now have petitioned Prime Minister Iraq’s Mohammed Shia’ Al Sudani to keep the internet on. They contend that the shutdowns do not control cheating and have not prevented the sharing of exam answers.

Prevention of cheating on exams is not the only reason for internet shutdowns in Iraq. Internet blackouts have been used during time of political unrest, particularly as **a tool to stifle protests**. This behavior by governments certainly isn't unprecedented. In an article in TechPolicy.Press, Ramsha Jahangir reports that 2024 was one of the worst years for internet shutdowns, with most in happening in a small group of countries:

> The majority of internet shutdowns were experienced by people in four countries: India, Myanmar, Pakistan, and Russia, where millions lived through at least 203 shutdowns — 69% of the global total. In Myanmar, at least six perpetrators, led predominantly by the regime, imposed 85 shutdowns across the country, reflecting the unprecedented scale at which shutdowns have been deployed since the outbreak of civil war in 2021. Following closely behind was India with 84 shutdowns.

**India is usually the worst offender** for these shutdowns (according to the report, they lost that privileged status to Myanmar in 2024). In total, 294 internet blackouts occurred in over 50 counties.

Not all internet shutdowns are purposeful or sinister. In 2024, Chad experienced a two-day outage that was caused when a fiber optic cable was damaged in a flood. The internet infrastructure in Chad has serious resiliency issues, and shutdowns have cost millions in GDP loss. In other cases, cables from Sudan and Cameroon have been cut, the former due to ongoing conflict in that country.

## How Do Internet Shutdowns Work?

### Border Gateway Protocol

The internet is not one big network. Instead, it is a collection of smaller networks or groups of networks called **autonomous systems (AS)**. So one internet service provider, or even a country, may have a group of networks and IP addresses that they control. These autonomous systems need to be able to communicate with each other. This is done using the **Border Gateway Protocol (BGP).** The BGP is responsible for routing information between one AS and another. If a government wants to completely stop the flow of information, they could block the BGP, preventing IP addresses from that AS from accessing the internet. This was done in Egypt in 2011.

### Packet Filtering and DPI

Since a disrupted BGP is easy to detect (more on this later), some governments instead use packet filtering. This technique involves blocking websites such as Facebook, X, or Instagram. **Deep packet inspection (DPI)** is more sophisticated, and allows very granular monitoring of packets. According to Fortinet:

> **Deep packet inspection (DPI), also known as packet sniffing, is a method of examining the content of data packets as they pass by a checkpoint on the network.** With normal types of stateful packet inspection, the device only checks the information in the packet’s header, like the destination Internet Protocol (IP) address, source [IP address](https://www.fortinet.com/resources/cyberglossary/what-is-ip-address), and port number. DPI examines a larger range of metadata and data connected with each packet the device interfaces with. In this DPI meaning, the inspection process includes examining both the header and the data the packet is carrying.

DPI has benefits over complete internet blackouts or the blocking of large blocks of IP addresses. Specific types of threats and information can be targeted with DPI instead of the whole internet.

### Brute Force Attacks and Other Methods

More brute force methods of cutting internet services include simply cutting power to data centers or cutting fiber optic cables. This would be particularly effective in regions with already poor infrastructures and would cause a total loss of connectivity. Other methods include:

- **Throttling**, where the a government mandates that ISPs slow internet speeds (This has been done in Russia)
    
- **Blocking specific URL's or IP addresses** like facebook.com
    
- **Denial of Service (DoS)** attacks to simply overwhelm networks or websites
    
- **DNS blocking**
    

## How Are Internet Shutdowns Detected?

Detecting internet shutdowns is not an exact science. It often relies on indirect techniques like "side-channel attacks", on the analysis of data from internet users in affected areas, or from probes. The probes can be blocked, and it isn't always easy to differentiate purposeful shutdowns versus infrastructure failures. That having been said, it is still possible to get a lot of useful information and to make accurate inferences.

### Internet Outage Detection and Analysis (IODA)

IODA is a project of the Georgia Institute of Technology Internet Intelligence Lab. Their goal is to measure internet outages affecting communication between autonomous systems or that affect specific regions of a country. IODA uses three measurements to detect outages:

- **Active Probing** which probes a network in a location to determine of it is active
    
- **Border Gateway Protocol** as described above
    
- **Telescope** which measures "unsolicited internet traffic", kind of like the noise pollution of the internet.
    

IODA then charts and analyses this data to give an accurate picture of the flow of information (or lack of flow) in a specific AS or region. So, looking at a graph of the last week in Iraq shows the following:

![IODA chart](/images/ioda.png)

You can see that at both exam times, all the parameters drop off sharply. The BGP is a very good measure of purposeful internet outages as it is usually very stable, even when other parameters fluctuate. This data can also be correlated with known action by the **Iraqi Ministry Of Education**, so is verifiable. IODA measures connectivity in all or most countries that have internet services, although there are reports of IODA being blocked in rigidly controlled countries such as China. IODA also has APIs that allow researchers to automate searches (see https://api.ioda.inetintel.cc.gatech.edu/v2/).

### Open Observatory of Network Interference (OONI)

Like IODA, OONI is a free resource for tracking internet censorship around the world run by the **Tor Project**. OONI collects data from tests run by volunteers around the world. The volunteers download the OONI probe which tests their networks for any form of blocking. Searches on OONI can be broken down by AS, region, time period, domain, country, or website. The probe runs on iOS, Android, Windows, and MacOS.

OONI also provides a **Measurement Aggregation Toolkit (MAT)** that allows users to generate custom charts "based on **aggregate views of real-time OONI data** collected from around the world." The data from MAT can be downloaded as JSON or CSV data, or viewed as a chart. The following screenshot is from a search I did on OONI for censorship of news media in Myanmar for a week in May 2025. It shows **DNS tampering** affecting many sites (this shows part of a multi-page result):

![OONI chart](/images/ooni.png)

This doesn't show a total internet outage, as the MAT for this data showed fair overall website connectivity for this period. It does, though, demonstrate how the tool can be used to drill down and look at the data.

### Censored Planet

Censored Planet is another organization that provides a dashboard for analyzing internet outages and censorship data from 200 countries. I did a search of blocking of human rights and news media in Belarus for May 10-May 23, 2025 and got the following results:

![CP Image](/images/cf.png)

The table shows 100% "Unexpected Results" for **Human Rights Watch** and T**he Organized Crime and Corruption Reporting Project (OCCRP)**. This isn't surprising as both human rights and government corruption are heavily censored topics in both Belarus and Russia.

### Cloudflare

One last resource I'd like to mention is **Cloudflare Radar.** According to their website

> Cloudflare Radar is a hub that showcases global Internet traffic, attack, and technology trends and insights. Cloudflare Radar is powered by data from Cloudflare's global network, as well as aggregated and anonymized data from Cloudflare's 1.1.1.1 public DNS Resolver...
> 
> With Radar, you can access trends and insights, like the adoption of new technologies, browsers or operating systems. Radar also keeps up to date with relevant events around the world to provide information on Internet activity patterns.

I used this service to analyze the same Iraqi internet shutdowns shown by IODA above:

![Cloudflare](/images/cf2.png)

Cloudflare Radar is geared toward **threat tracking and cybersecurity**, but it is still a useful tool for tracking internet outages. It isn't quite as granular as IODA, OONI, and Censored Planet but benefits from Cloudflare's global network.

## What Are the Consequences of Internet Shutdowns?

When internet service to a country or region is restricted or shut down. the flow of information is cut off, both between citizens and from the outside world. If citizens can't communicate online, **organizing any kind of protest is difficult or impossible**. Governments know this and frequently cut off internet access when they get wind of protesters organizing. According to the **World Economic Forum (WEF),** internet shutdowns are like a kind of "collective punishment. Even worse, **access to medical care** and **online education** are blocked.

The WEF warns of another impact of internet shutdowns. When the government restricts access to information, they create an **"echo chamber"** where only government sanctioned information circulates. This isolation from the rest of society creates a "path of least resistance for **misinformation, disinformation** and **malicious propaganda...**" On top of all these other impacts, internet shutdowns create a roadblock to doing business. Myanmar's record-breaking internet shutdowns have cost the country $2.8 Billion, with many more billions being lost in other heavily regulated countries.

So, internet shutdowns, whether used as a tool by repressive regimes or the result of crumbling infrastructure, conflict, or sabotage, impact freedom to organize, do business, access accurate information, and even to access life-saving services. These outages are being monitored by several different organizations including IODA, OONI, Censored Planet, Cloudflare and others. The goal of these groups is to provide researchers and journalists with accurate and up-to-date information about censorship throughout the world.

## Sources

https://ioda.inetintel.cc.gatech.edu/dashboard

https://pulse.internetsociety.org/blog/iraq-to-shutdown-internet-during-2025-exam-period

https://therecord.media/iraq-school-exams-internet-blackouts

https://www.techpolicy.press/why-2024-was-the-worst-year-for-internet-shutdowns/

https://www.voaafrica.com/a/internet-outage-paralyzes-business-in-chad-outrage-grows/7827070.html

https://pulse.internetsociety.org/en/blog/chads-recent-outages-highlight-resiliency-gaps

https://www.fortinet.com/resources/cyberglossary/dpi-deep-packet-inspection

https://theconversation.com/internet-shutdowns-heres-how-governments-do-it-211081

https://www.weforum.org/stories/2022/10/internet-shutdowns-explainer/

[https://explorer.ooni.org/chart/mat?test\\\_name=web\\\_connectivity&axis\\\_x=measurement\\\_start\\\_day&since=2025-04-24&until=2025-05-24&time\\\_grain=day](https://explorer.ooni.org/chart/mat?test%5C_name=web%5C_connectivity&axis%5C_x=measurement%5C_start%5C_day&since=2025-04-24&until=2025-05-24&time%5C_grain=day)

https://explorer.ooni.org/

https://www.weforum.org/stories/2022/10/internet-shutdowns-explainer/
