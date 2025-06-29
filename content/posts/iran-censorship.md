---
title: "Iran: The Disconnected Nation"
date: 2025-06-23T08:50:11-04:00
draft: false
---

## The mechanisms and human cost of Iran's censorship regime.

![iranian-flag](images/iran-flag.jpg)

## The Israel-Iran Conflict Highlights Lack of Freedom in Iran

I've been wanting to write a post about online censorship in Iran for a while. With the week-old onset of a major conflict between Iran and Israel, this seemed like a good time to do it. Iran has a very sophisticated censorship system. The entire system, including an advanced technical infrastructure, the Iranian legal system, and aggressive repression of any dissident voices, makes the Iranian censorship apparatus second only to China in its scope (for more information about China's Great Firewall, see the post [Golden Shield: The Inner Workings of China's Great Firewall](https://merecat25.substack.com/p/golden-shield-the-inner-workings?r=l5bk9)).

## Control of Iran's Internet Space

The censorship apparatus in Iran is highly centralized. Authority to oversee all information that reaches the population is controlled by the:

- Supreme Leader **Ayatollah Ali Khamenei**
- **Supreme Council of Cyberspace** (controlled directly by the Supreme Leader)
- **Revolutionary Guard**, for enforcement
- **Ministry of Information and Communication Technology,** which controls the implementation of internet censorship
- **Internet Filtering Committee (IFC)**, which decides the websites and protocols that are blocked or allowed
- **Telecommunication Company of Iran (TCI)**, responsible for state-controlled ISPs

Internet censorship in Iran started early, with the revolutionary government seeing the internet as a tool for spreading information. This morphed into increasing restrictions and the eventual establishment of an "Intranet" as a substitute for the world wide web in the second decade of the 21st century. This intranet is known as the **National Information Network (NIN)**.  NIN expanded on a preexisting law, the **Computer Crimes Law (CCL)** of 2010, that restricted encryption and required ISP's to track internet use and retain personal information of citizens. In keeping with the country's terrible record on human rights, gender specific restrictions have also been put in place. According to the Miaan Group:

> Facial recognition and other surveillance technologies are utilized to enforce social norms, particularly targeting women. Applications such as the *Nazer* app enable citizens to report perceived violations, furthering the state’s surveillance capabilities. Content creators, especially women, face increased scrutiny and pressure to conform to state-imposed moral standards.

Religious and LGBTG+ groups face the same scrutiny and restrictions.

## Methods of Online Censorship in Iran

The implementation of internet censorship in Iran covers all of the bases. **Deep packet inspection (DPI)**, allows authorities to analyze all online traffic and filter it based on such factors as:

- VPN use
- Illegal encryption
- Use of Censorship circumvention tools like Tor, Psiphon, and proxy servers

Iran actually uses a protocol **whitelisting** system. The opposite of blacklisting, whitelisting determines which protocols are actually allowed. In Iran, this includes a relatively small number of approved protocols and sites. On the NIN, if it isn't explicitly allowed, it is filtered. Sites and protocols are blocked or sometimes "throttled" which slows internet traffic enough to make accessing sites or using protocols like TOR impractical.

The NIN itself is a system of switches, routers, data centers, and protocols which prevents any internet traffic from entering or leaving Iran. To use either the public sector or government sector of the intranet, users must give a telephone number and a **social ID number**. Any internet service providers must implement standards for web traffic and email approved by the central government authorities. If they don't, they get shut down (and many have been). The TCI decides how ISPs operate and whether they can continue to do so. For more in depth information about how DPI and other technological censorship methods work, see [Censored Lab](https://merecat25.substack.com/p/censored-lab) or our website at [censored-state.com](https://censored-state.com/).

## Example of an Internet Shutdown in Iran

After the conflict with Israel began about a week ago, it appears that Iran's censorship apparatus swung into full gear. Several ISPs show a **complete internet shutdown**. Looking at the IODA data from the Gilass Rayaneh region, we see:

![ioda-chart](/images/iran1.png)

At the 9:00 PM mark the **Border Gateway Protocol BGP** drops to zero. BGP is how different networks communicate with each other. This graph likely shows a complete loss of contact with the outside world or even with neighboring regions inside of Iran. While conflicts can damage fiber optic cables, this is most likely a deliberate internet shutdown designed to prevent access to information about the ongoing conflict. This is the ultimate way to control the narrative: simply make getting information impossible. We can cross-reference the outage on Cloudflare:

![cloudflare](/images/iran2.png)

This graph from Iran shows a complete drop off of **HTTP requests** starting late on Wednesday June 18th. Keep in mind, services like Facebook, Signal, and TOR are almost always blocked or throttled in Iran anyway. Below is a typical graph from OONI showing blocking of Signal. This is a daily finding in Iran:

![ooni-chart](/images/iran3.png)

The total shutdown, on the other hand, seems to be an emergency measure.

## Consequences of Violating Laws Regulating Online Activity

Penalties for violating laws restricting online activity can be severe. Extended jail sentences can be the consequence for using the internet for self expression or political opposition. Take, for example, the cased of Hossein Shanbehzadeh. This activist, writer and blogger was arrested in 2024 and sentenced to 12 years in prison. His crime consisted of responding to a tweet by Supreme Leader Ali Khamenei with a single dot. Charges included insulting Islam and spreading "pro-Israel" propaganda. He isn't the first or only blogger to be arrested in Iran. Other bloggers have faced stiff sentences for "espionage" and for being linked to "foreign intelligence services." And bloggers aren't the only ones targeted. According to vice.com:

> Shanbehzadeh is just one of the countless activists who have received harsh retribution for their words and actions. In an attempt to control the disapproving messages from artists and public figures, the Iranian government has become relentless with its prison sentences. In fact, one individual, an Iranian rapper named [Toomaj Salehi](https://www.npr.org/2024/04/30/1248245576/iranian-rapper-receives-death-sentence-for-songs-criticizing-the-establishment), even received the death sentence for creating anti-government videos.

## The Wider Consequences of Iranian Internet Censorship

The citizens of Iran are cut off from the modern world by a combination of external sanctions and repressive internet restrictions. External sanctions are outside the scope of this post, but the combination isolates the internet users of Iran, whether regular citizens, students, or business owners. Lack of access to the global economy negatively affects the GDP of Iran. The poor quality of the Iranian internet limits the scope of information available to citizens. Students can't access educational resources. Research and innovation are stifled. Organizing protests and opposition to government policies is difficult if not impossible. The Iranian censorship apparatus controls all the channels of communication and exposure to any alternative worldviews, to the detriment of its citizens and economy.

## Sources

https://ioda.inetintel.cc.gatech.edu/asn/200645-IR

https://explorer.ooni.org/

https://en.wikipedia.org/wiki/Internet_censorship_in_Iran

https://miaan.org/irans-digital-control-the-evolution-of-censorship-and-surveillance-amidst-the-women-life-freedom-movement/

https://www.vice.com/en/article/iran-writer-hossein-shanbehzadeh-period-prison-twitter/

https://radar.cloudflare.com/

https://en.wikipedia.org/wiki/National_Information_Network

https://www.internetgovernance.org/2024/10/22/caught-in-the-crossfire-the-impact-of-sanctions-on-iranian-internet-users/