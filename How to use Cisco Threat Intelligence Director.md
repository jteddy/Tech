# How to use Cisco Threat Intelligence Director on the Firepower Management Center

The opinions expressed in this blog are my own views and not those of Cisco.

# Introduction
![Threat Intelligence Director](https://i.imgur.com/3dVM3FE.png)
Cisco's Threat Intelligence Director runs on Cisco's Firepower Management Center. It is used to ingest threat intelligence using open standards. Firepower sensors (essentially Cisco's NGFW or NGIPS) provide a rich source of information that includes host and user information, traffic flows from source & destination IP's, port and protocol. Utilising this existing information and leveraging the Threat Intelligence Director you are able to expand your security effectiveness by detecting actionable indicators of compromise from those threat feeds.

## What is the Firepower Management Center
![FMC](https://i.imgur.com/L8MfVPt.png)
The Firepower Management Center provides unified management of Cisco's NGFW and NGIPS known as Firepower Threat Defence.

## Why Use Threat Intelligence Director?

Cyber Security teams are typically ingesting threat intelligence from multiple sources. They then try to operationalise that threat intelligence information in their environment which can present challenges. By using the Threat Intelligence Director on the Firepower Management Center you are able to leverage threat intelligence information leveraging the existing intelligence in Firepower Management Center.

# Threat Intelligence Sources
## Industry Organisations
There are a number of commercial organisations that provide threat intelligence information for specific industries. Some of these organisations offer threat feeds but may not support STIX/TAXII.

---

**[Cyber Threat Alliance](https://www.cyberthreatalliance.org/)** - The CTA is a not-for-profit organization that is working to improve the cybersecurity of our global digital ecosystem by enabling near real-time, high-quality cyber threat information sharing among companies and organizations in the cybersecurity field. 

![CTA Members](https://i.imgur.com/QKqslAD.png)

---

[**Financial Services Information Sharing and Analysis Center**](https://www.fsisac.com/) - FS-ISAC was established by the financial services sector in response to government directives mandating that the public and private sectors share information about physical and cyber security threats to help protect the U.S. critical infrastructure.

---

**[R-CISC](https://r-cisc.org/)** - Cyber intelligence sharing for the for the Retail Industry.  Membership fees are based on the companies revenue.

## Open Source STIX/TAXII Threat Intelligence Feeds

I found a one-stop shop for a list of threat intelligence feeds on GitHub by Herman Slatman aka [hslatman](https://github.com/hslatman) titled [awesome-threat-intelligence](https://github.com/hslatman/awesome-threat-intelligence). This begs the ultimate question. Which threat feed will give me the most value. With so many open source threat feeds do I still need to procure a threat intelligence feed from a vendor? Further analysis is required to answer this question.

The below open source feeds look like a great starting point providing STIX/TAXII support.
- [AlienVault Open Threat Exchange (OTX)](https://www.alienvault.com/open-threat-exchange)
- [Hail A Taxii](http://hailataxii.com)

## Open Source Threat Intelligence Feeds

These feeds may not provide native STIX/TAXII support so you have two options. Build your own server that downloads the feeds, normalise/parse that feed then provides the data to FMC via your own TAXII server. Procure a threat intelligence platform that utilises the feeds you require and provides you with a STIX/TAXII feed.

- [Abuse.ch Trackers](https://abuse.ch/)
- [Bambenek Source](http://osint.bambenekconsulting.com/feeds)
- [Blocklist.de](http://www.blocklist.de/)
- [BotScout Bot List](http://botscout.com/)
- [Botvrij](https://botvrij.eu/)
- [BruteForceBlocker](http://danger.rulez.sk/index.php/bruteforceblocker)
- [CI Army List](https://app.threatconnect.com/auth/ciarmy.com)
- [Cryptam](http://www.malwaretracker.com/)
- [Dan.me Tor List](https://www.dan.me.uk/)
- [GreenSnow Blocklist](https://greensnow.co/)
- [ETOpen Compromised IPs](http://emergingthreats.net/open-source/etopen-ruleset/)
- [Hybrid Analysis](https://www.hybrid-analysis.com/)
- [Liste Malware](http://malwaredb.malekal.com/)
- [Malshare Daily Malware List](http://malshare.com./)
- [Malware Domain Blocklist](http://www.malwaredomains.com/)
- [Malware Domain List](http://www.malwaredomainlist.com/)
- [PDF Examiner](http://www.phishtank.com/index.php)
- [PhishTank](http://www.phishtank.com/index.php)
- [Rutgers Attacker IPs](http://report.cs.rutgers.edu/mrtg/index.html)
- [SARVAM](http://sarvam.ece.ucsb.edu/)
- [ThreatExpert](http://www.threatexpert.com/)
- [ViruSign](http://www.virusign.com/)
- [VX Vault Source](http://vxvault.net/)
- [WSTNPHX Malware Email Addresses](https://github.com/WSTNPHX/scripts-n-tools/blob/master/malware-email-addresses.txt)

## Threat Intelligence Vendors & Platforms 

A threat intelligence feed may be included in a security product you procure from a vendor an additional paid subscription or an entire threat intelligence platform.  Some players in the commercial market are:

- [Accenture iDefense](https://www.accenture.com/us-en/service-idefense-security-intelligence)
- [Anomali](https://www.anomali.com/)
- [Booz Allen Hamilton Cyber4Sight](https://www.boozallen.com/s/product/cyber4sight.html)
- [Cofense Intelligence](https://cofense.com/product-services/phishing-intelligence/)
- [Cisco Talos](https://www.talosintelligence.com/)
- [Crowdstrike Faalcon Intelligence](https://www.crowdstrike.com/solutions/threat-intelligence-solutions/)
- [Department of Homeland Security](https://www.dhs.gov/ais)
- [Digital Shadows](http://digitalshadows.com/our-solutions/digital-shadows-searchlight-/)
- [EclesticIQ](https://www.eclecticiq.com)
- [Flashpoint](https://www.flashpoint-intel.com/)
- [FireEye iSIGHT Intelligence Subscription](https://www.fireeye.com/solutions/isight-cyber-threat-intelligence-subscriptions.html)
- [Infoblox](https://www.infoblox.com/)
- [Intel 471](https://intel471.com/)
- [Kapersky](https://www.kaspersky.com.au/enterprise-security/cybersecurity-services)
- [Lookingglass](https://www.lookingglasscyber.com/)
- [Recorded Future RiskList](https://www.recordedfuture.com/)
- [Symantec DeepSight](https://www.symantec.com/services/cyber-security-services/deepsight-intelligence)
- [TheatQuotient](https://www.threatq.com/)
- [ThreatConnect](https://www.threatconnect.com/)

# Threat Intelligence Director Configuration

## Requirements

The Threat Intelligence Director requires Firepower Management Center to have:

- over 15GB of RAM to operate
- FMC and FTD running version 6.2.2 or later
- REST API Access enabled 
  - Enable the REST API. *System -> Configuration -> Rest API Preferences*

![FMC Rest API Setting](https://i.imgur.com/1XNcgTk.png)

The [Firepower Management Center Configuration Guide, Version 6.2.3](https://www.cisco.com/c/en/us/td/docs/security/firepower/623/configuration/guide/fpmc-config-guide-v623/cisco_threat_intelligence_director__tid_.html?bookSearch=true) states there is no performance impact on managed devices.

If you host TID on the active Firepower Management Center in a high availability configuration, the system does not synchronize TID configurations and TID data to the standby Firepower Management Center. We recommend performing regular backups of TID data on your active Firepower Management Center so that you can restore the data after failover. 

## Threat Intelligence Director Terminology

### Indicators and Observables

*Observables* is an IOC. The IOC can be a:

- SHA-256

- Domain
- URL
- IPv4
- IPv6
- Email To
- Email From
- Email Sender
- Email Subject

![Observables Screenshot in FMC](https://i.imgur.com/F5iqyKd.png)

  

| Observables Bar | Definition                                                   |
| :-------------: | ------------------------------------------------------------ |
|      Type       | The IOC. See the list of supported IOCs above                |
|      Value      | The value or string of the IOC Type                          |
|   Indicators    | The number of Indicators using this Observable               |
|     Action      | The action of the observable. This can be Monitor or Block   |
|     Publish     | Pause or enable publishing of observables to elements        |
|   Updated At    | The last time the observable was updated                     |
|     Expires     | The timestamp of when the Observable will expire             |
|    Whitelist    | That icon at the very end allows you to add or remove the Observable from a whitelist |

After the *Observables* are published to the elements, the elements monitor traffic and report observations to the Firepower Management Center when the system identifies *Observables* in traffic.

*Indicators* contain one or more *Observables*. *Indicators* convey all the IOCs associated with a known threat. An Incident is created by the Threat Intelligence Director and an event is generated in FMC if an observable meets the conditions of an *Indicator*.

Upon adding the source, you cant change the whole feed to block. This has to be done individually on *Indicators* and *Observables*.

### Elements

An *Elements* is a managed Firepower Threat Defense device.

![](https://i.imgur.com/DyrSBDd.png)

### Licensing

TID requires the Threat Defence License for Security Intelligence Filtering. A Malware license is required to configure file policies for SHA-256 observable publishing.

Source: [https://www.cisco.com/c/en/us/td/docs/security/firepower/623/configuration/guide/fpmc-config-guide-v623/licensing_the_firepower_system.html?bookSearch=true﻿](https://www.youtube.com/redirect?redir_token=anp7dRztRvZs9KiKkAnTcRI7s6t8MTUzMTY1NDIwMUAxNTMxNTY3ODAx&q=https%3A%2F%2Fwww.cisco.com%2Fc%2Fen%2Fus%2Ftd%2Fdocs%2Fsecurity%2Ffirepower%2F623%2Fconfiguration%2Fguide%2Ffpmc-config-guide-v623%2Flicensing_the_firepower_system.html%3FbookSearch%3Dtrue&event=comments) 

### STIX Requirements

STIX Files must be STIX Version 1.0, 1.1, 1.1.1, or 1.2 and adhere to the guidelines in the STIX documentation: http://stixproject.github.io/documentation/suggested-practices/. STIX files can include complex indicators. 

### Flat File Upload Requirements

Files must be ASCII text files with one observable value per line. Each file should contain only one type of content: 

- SHA-256 - SHA-256 hash values.

- Domain - Domain names as defined in RFC 1035.
- URL - URLs as defined in RFC 1738.
- IPv4— IPv4 addresses as defined in RFC 791. (CIDR blocks are unsupported)
- IPv6— IPv6 addresses as defined in RFC 4291.

## Ingesting Feeds

*Intelligence -> Sources -> Add Source*

Threat Intelligence Director supports the following feeds:

- STIX/TAXII
- URL (STIX or Flat File)
- File Upload (STIX or Flat File)

The minimum update time of a feed is 30 minutes imposed by FMC.

![](https://i.imgur.com/SjGZaWQ.png)

### AlienVault Open Threat Exchange (OTX)

1. Create an account and login to [AlienVault OTX](https://otx.alienvault.com/)
2. Click on *API*
   1. ![](https://i.imgur.com/YYOHnet.png)
3. Click on *TAXII*. You will be presented with the instructions on how to set up the STIX/TAXII feed.
4. The discovery URL is: https://otx.alienvault.com/taxii/discovery 
5. The username is your API key. This is your OTX Key. There is no password needed.
6. Go to Firepower Management Center and click on *Intelligence -> Sources -> Add Source (+)*
7. Enter in the details and you should see the selectable feed populate.
   1. ![](https://i.imgur.com/h4IsFs9.png)
8. Click on *Save*
9. You now have your feed added. At the time of this post, the feed had 5,233 indicators and 4,937 observables.
   1. ![](https://i.imgur.com/f5Mi82V.png)
10. The import did complete although with errors as some information was invalid or unsupported.
    1. ![](https://i.imgur.com/A1FGWO4.png)

### Hail A TAXII

1. Go to Firepower Management Center and click on *Intelligence -> Sources -> Add Source (+)*
2. Enter in the [Hail A TAXII](http://hailataxii.com/) discovery URL in the URL field. <http://hailataxii.com/taxii-discovery-service> 
3. A username and password is required. Use *guest/guest* 
4. Select your desired feeds.
   1. ![](https://i.imgur.com/3cQWflK.png)
5. Click on Save.

![](https://i.imgur.com/BnxwuLY.png)

### Threatconnect
I registered an account with Threatconnect at https://www.threatconnect.com/free/. The verification email took a few hours to arrive. 

One of my pet hates is for an email to arrive in my inbox with a password, even if it is randomly generated and requires me to change it on my first login.

![](https://i.imgur.com/knmAHBE.png)

You need to create your STIX/TAXII feed on Threatconnect. I completed these steps following the [CREATING AN OUTBOUND TAXII EXCHANGE FEED](https://kb.threatconnect.com/customer/en/portal/articles/2781866-creating-an-outbound-taxii-exchange-feed) on their website. I contacted their support team to find it if the guide was out of date.

### RiskIQ

I registered an account with RiskIQ to access their Community API. Unfortunately, it was exactly that. There was no STIX/TAXII feed for me to utilise. For those interested, you can create an account at  https://community.riskiq.com/registration.

### Critical Stack

From  [hslatman](https://github.com/hslatman)'s' [awesome-threat-intelligence](https://github.com/hslatman/awesome-threat-intelligence) I discovered [Critical Stack's](https://www.criticalstack.com/) free [intelligence feed](https://intel.criticalstack.com/). Critical Stack is a secure container orchestration platform.

1. Sign up at https://intel.criticalstack.com/user/sign_up
2. Critical Stack will email you with an activation link. You have three hours to click on the activation link.

Unfortunately, this platform only provided API access. At this stage, I gave up on some other sites that said they provided free feeds as there was no information if they supported STIX/TAXII. 


# Benefits of Threat Intelligence Director

- Detect and block *Indicators* and *Observables* with automated actions
- Accurate detection and incident reporting
- Improve your time to detection and time to respond to threats on your network
- Provides a single integration point for all STIX/TAXII and flat file intelligence sources

# Closing Thoughts

After writing this documentation I walked away with a few questions and thoughts:

- Does the threat protection I obtain in Cisco's Firepower devices protected by Cisco's threat intelligence research centre Cisco Talos provide me with adequate protection without the need for Threat Feeds? Another argument could be made that if you are using open source threat feeds wouldn't Talos be using them as well? Let us suppose they do I would imagine there is curation done before disseminating this information down into their products.
- Are organisations benefiting from using commercial threat feeds? If so what security technologies are they using for threat protection that utilize those feeds and how are they correlating intelligence to search for IOC's across their enterprise?
- How often are organisations looking for IOC's specific to their environment and what tools are they using to do so?
- Another how-to post I am considering is utilising threat feed API's to download the data, parse/normalise the data to create my own local STIX/TAXII feed for FMC/Threat Intelligence Director.
- I contemplated doing a bake-off to see which threat intelligence feeds detect threats sooner than the other and the efficacy of their IOC's published. The pitfall of doing this review is that I would only analyse a small sample size based on the time I have and a feed/source that scored poorly this month could score higher in the following month. 

# Definitions

FMC - Firepower Management Center

FTD - Firepower Threat Defense

STIX - Structured Threat Information eXpression

TAXII - Trusted Automated eXchange of Indicator Information

IOC - Indicator of Compromise

# Source Information

- [Firepower Management Center Configuration Guide, Version 6.2.3 - Cisco Threat Intelligence Director](https://www.cisco.com/c/en/us/td/docs/security/firepower/623/configuration/guide/fpmc-config-guide-v623/cisco_threat_intelligence_director__tid_.html?bookSearch=true)
- [YouTube - Cisco Firepower Threat Defense 6 2 2: Threat Intelligence Director (Hail A TAXII)](https://www.youtube.com/watch?v=0usmyIrA0fA&t=95s)
- [YouTube - Cisco Firepower Threat Defense 6 2 2 : Threat Intelligence Director (Flat File)](https://www.youtube.com/watch?v=s-laX74reXo)
- [YouTube - CTID](https://www.youtube.com/watch?v=DsOkgB8BFzk)
