---
title: "IP Address Privacy Considerations"
abbrev: "IP Address Privacy Considerations"
docname: draft-ip-address-privacy-considerations-latest
category: info

ipr: trust200902
area: General
workgroup: Network Working Group
keyword: Internet-Draft

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: M. Finkel
    name: Matthew Finkel
    organization: The Tor Project
    email: sysrqb@torproject.org
 -
  ins: L. Iannone
  name: Luigi Iannone
  org: Huawei Technologies France S.A.S.U
  abbrev: Huawei
  email: luigi.iannone@huawei.com


normative:

informative:

  I-D.pauly-dprive-oblivious-doh:
  I-D.thomson-http-oblivious:

  WEBTRACKING1: DOI.10.1109/JPROC.2016.2637878
  WEBTRACKING2: DOI.10.1145/3366423.3380161
  VPNCMP1: DOI.10.15496/publikation-41810
  VPNCMP2: DOI.10.1109/MCOM.2004.1341273
  GEOIP: DOI.10.1145/3442442.3451888
  TOR:
    title: The Tor Project
    target: https://www.torproject.org/
  VPNTOR:
    title: "Anonymity communication VPN and Tor: A comparative study"
    author:
      ins: E. Ramadhani
    target: Journal of Physics Conference Series
  GNATCATCHER:
    title: "Global Network Address Translation Combined with Audited and Trusted CDN or HTTP-Proxy Eliminating Reidentification"
    target: https://github.com/bslassey/ip-blindness
  APPLEPRIV:
    title: "Apple iCloud Private Relay"
    target: https://appleinsider.com/articles/21/06/10/how-apple-icloud-private-relay-works

--- abstract

This document provides an overview of privacy considerations related to user IP addresses. It includes an analysis of some current use cases for tracking of user IP addresses, mainly in the context of anti-abuse. It discusses the privacy issues associated with such tracking and provides input on mechanisms to improve the privacy of this existing model. It then captures requirements for proposed 'replacement signals' for IP addresses from this analysis. In addition, existing and under-development techniques are evaluated for fulfilling these requirements.


--- middle

# Introduction

The initial intention of this draft is to capture an overview of the problem space and research on proposed solutions concerning privacy considerations related to user IP addresses. The draft is likely to evolve significantly over time and may well split into multiple drafts as content is added.

Tracking of user IP addresses is common place on the Internet today, and is particularly widely used in the context of
anti-abuse, e.g. anti-fraud, DDoS management child protection activities. IP addresses are currently used as a source of
“reputation” in conjunction with other signals to protect against malicious traffic, since they are a relatively stable
identifier of the origin of a request. Servers use these reputations in determining whether or not a given packet, connection,
or flow corresponds to malicious traffic.

However, identifying the activity of users based on IP addresses has clear privacy implications ({{WEBTRACKING1}}, {{WEBTRACKING2}}), e.g. user fingerprinting and cross site identity linking. Many technologies exist today to allow users to hide their IP address to avoid such tracking, e.g. VPNs ({{VPNCMP1}}, {{VPNCMP2}}) or Tor ({{TOR}}, {{VPNTOR}}). Several new technologies are also emerging in the landscape e.g. Gnatcatcher {{GNATCATCHER}}, Apple Private Relay {{APPLEPRIV}} and Oblivious technologies (OHTTP {{I-D.thomson-http-oblivious}}, ODoH {{I-D.pauly-dprive-oblivious-doh}}).

General consideration about privacy for Internet protocols can be found in {{!RFC6973}}. This document is more specific and attempts to capture the following aspects of the tension between valid use cases for user identification and the related privacy concerns including:

* An analysis of the current use cases, attempting to categorize/group such use cases where commonalities exist
* Find ways to enhance the privacy of existing uses of IP addresses.
* Generating requirements for proposed 'replacement signals' from this analysis (these could be different for each category/group of use cases)
* Research to evaluate existing technologies or propose new mechanisms for such signals

# Terminology

(Work in progress)

- Reputation: A random variable with some distribution. A reputation can either be "bad" or "good" with some probability according to the distribution.
- Reputation signal: A representative of a reputation.
- Reputation proof: A non-interactive zero knowledge proof of a reputation signal.
- Reputation context: The context in which a given reputation applies.
- Identity: Any identifying information about an end-user or service, be it a client or server, including IP addresses.

# IP address tracking

## IP address use cases

### Anti-abuse

Cyber-attackers abuse IP addresses posing a serious risk since legitimate service providers, developers, and end users may be mistakenly blacklisted which lowers the image and hurts the reputation of the service.

Account abuse, financial fraud, ad fraud, child abuse...



### DDoS and Botnets

Cyber-attackers can leverage on the good reputation of an IP address to carry out specific attacks that wouldn't work otherwise. Main examples are Distributed Denial of Service (DDoS) attacks carried out spoofing a trusted (i.e., having good reputation) IP address (which may or may not be the victim of the attack) so that the servers used to generate the DDoS traffic actually respond to the attackers trigger (i.e., spoofed packets). Similarly Botnets may use spoofed addresses in order to gain access and attack services that would not be otherwise reachable.     

## Privacy implications of IP addresses

IP addresses are sent in clear throughout the packet journey over the Internet.
As such, any observer along the path can pick it up and use it for various tracking purposes. Beside basic information about the network or the device, it is possible to associate an IP address to an end user, hence, the relevance of of IP addresses for user privacy. A very short list of information about user, device, and network that can be obtained via the IP address.

- Determine who owns and operates the network. Searching the WHOIS database using an IP address can provide a range of information about the organization to which the address is assigned, including a name, phone number, and civic address;
- Through a reverse DNS lookup and/or traceroute the computer name can be obtained, which often contains clues to logical and physical location;
- Geo-localisation of the device (hence the user) through various techniques {{GEOIP}}. Depending on the lookup tool used, this could include country, region/state, city, latitude/longitude, telephone area code and a location-specific map;
- Search the Internet using the IP address or computer names. The results of these searches might reveal peer-to-peer (P2P) activities (e.g., file sharing), records in web server log files, or glimpses of the individual's web activities (e.g., Wikipedia edits). These bits of individuals’ online history may reveal their political inclinations, state of health, sexuality, religious sentiments and a range of other personal characteristics, preoccupations and individual interests;
- Seek information on any e-mail addresses used from a particular IP address which, in turn, could be the subject of further requests for subscriber information.

## IP Privacy Protection and Law

This section aim at providing some basic information about main example of laws adopted worldwide and related to IP address privacy (usually these laws area by product of the broader user privacy protection).

Possible content (to focus only on technical IP address related aspects):

- GDPR (General Data Protection Regulation) - EUROPE: Europe considers IP addresses as personal identification information that should be treated like any other personal information e.g. social security number.
- The United States has opted for a different approach to data protection. Instead of formulating one all-encompassing regulation such as the EU’s GDPR, the US chose to implement sector-specific privacy and data protection regulations that work together with state laws to safeguard American citizens’ data.
- In 2020, China released the first draft of Personal Information Protection Law (PIPL). The PIPL is the equivalent of European GDPR and will have significant influence.
- Japan Protection of Personal Information (APPI) Act (recent changes put the act close to the GDPR model).

## Mitigations for IP address tracking

List of possible techniques to be briefly described:

- Mix Networks/TOR
- Address anonymization (e.g. {{GNATCATCHER}} and similar)
- VPNs
- Temporary addresses


# Replacement signals for IP addresses

Fundamentally, the current ecosystem operates by making the paths of a connection accountable for bad traffic, rather than the
sources of the traffic itself. This is problematic because paths are shared by multiple clients and are impermanent. Ideally,
clients could present proof of reputation that is separate from the IP address, and uniquely bound to a given connection.

## Requirements

In the following the requirements of reputation signals are listed. Note that by "client(s)" it is intended an end user device (e.g., a PC or a mobile phone), while by "server(s)" it is intended a device offering an Internet service, which belong to an organisation/company but is not a personal device.    

### General Requirements

The following requirements apply to reputation signals in general, independently from whether is the reputation of a client or a server.

- Reputation signals MUST NOT remain valid indefinitely. New reputation signals periodically must be obtained periodically.
- Reputation MUST NOT be transferable.
- Reputation signals MUST be bound to a context, and MUST NOT be transferrable across contexts.

### Client requirements

The following requirement are specific to clients.

- Clients MUST be able to request and present new reputation proofs on demand.
- A reputation signal MUST NOT be linkable to any identifying information for which the signal corresponds.
- Clients MUST be able to demonstrate good faith and improve reputation if needed.
- Clients MUST be able to dispute their reputation.
- Clients MUST be able to determine and verify the context in which a given reputation applies.

### Server requirements

The following requirement are specific to servers.

- Servers MUST be able to request and present new reputation proofs on demand.
- Servers MUST be able to demonstrate good faith and improve reputation if needed.

## Evaluation of existing technologies

## Potential new technologies


# Security Considerations

TODO

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO
