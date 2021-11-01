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
    ins: B. Lassey
    name: Bradford Lassey
    organization: Google
    email: lassey@chromium.org
 -
  ins: L. Iannone
  name: Luigi Iannone
  org: Huawei Technologies France S.A.S.U
  abbrev: Huawei
  email: luigi.iannone@huawei.com
 -
    ins: J. B. Chen
    name: J. Bradley Chen
    organization: Google
    email: bradchen@google.com
 

normative:

informative:

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
  TRUSTTOKEN:
    title: "Trust Token API Explainer"
    target: https://github.com/WICG/trust-token-api
  WEBAUTHN:
    title: "Web Authentication: An API for accessing Public Key Credentials Level 2"
    target: https://www.w3.org/TR/webauthn-2/

--- abstract

This document provides an overview of privacy considerations related to user IP addresses. It includes an analysis of some current use cases for tracking of user IP addresses, mainly in the context of anti-abuse. It discusses the privacy issues associated with such tracking and provides input on mechanisms to improve the privacy of this existing model. It then captures requirements for proposed 'replacement signals' for IP addresses from this analysis. In addition, existing and under-development techniques are evaluated for fulfilling these requirements.


--- middle

# Introduction

The initial intention of this draft is to capture an overview of the problem space and research on proposed solutions concerning privacy considerations related to user IP addresses (informally, IP privacy). The draft is likely to evolve significantly over time and may well split into multiple drafts as content is added.

Tracking of IP addresses is common place on the Internet today, and is particularly widely used in the context of
anti-abuse, e.g. anti-fraud, DDoS management, and child protection activities. IP addresses are currently used in determining
"reputation" {{!RFC5782}} in conjunction with other signals to protect against malicious traffic, since these addresses are usually a relatively stable
identifier of a request's origin. Servers use these reputations in determining whether or not a given packet, connection,
or flow likely corresponds to malicious traffic. In addition, IP addresses are used in investigating past events and attributing responsibility.

However, identifying the activity of users based on IP addresses has clear privacy implications ({{WEBTRACKING1}}, {{WEBTRACKING2}}), e.g. user fingerprinting and cross-site identity linking. Many technologies exist today that allow users to obfuscate their external IP address to avoid such tracking, e.g. VPNs ({{VPNCMP1}}, {{VPNCMP2}}) and Tor ({{TOR}}, {{VPNTOR}}). Several new technologies are emerging, as well, in the landscape, e.g. Apple iCloud Private Relay {{APPLEPRIV}}, Gnatcatcher {{GNATCATCHER}}, and Oblivious technologies (ODoH {{?I-D.pauly-dprive-oblivious-doh}}, OHTTP {{?I-D.thomson-ohai-ohttp}}).

General consideration about privacy for Internet protocols can be found in {{!RFC6973}}. This document builds upon {{!RFC6973}} and more specifically attempts to capture the following aspects of the tension between valid use cases for user identification and the related privacy concerns, including:

* An analysis of the current use cases, attempting to categorize/group such use cases where commonalities exist.
* Find ways to enhance the privacy of existing uses of IP addresses.
* Generating requirements for proposed 'replacement signals' from this analysis (these could be different for each category/group of use cases).
* Research to evaluate existing technologies or propose new mechanisms for such signals.

# Terminology

(Work in progress)

This section defines basic terms used in this document, with references to pre-existing definitions as appropriate. As in {{!RFC4949}} and {{!RFC6973}}, each entry is preceded by a dollar sign ($) and a space for automated searching.

- $ Identity: Extending {{!RFC6973}}, an individual's attributes may only identify an individual up to an anonymity set within a given context.
- $ Reputation: A random variable with some distribution. A reputation can either be "bad" or "good" with some probability according to the distribution.
- $ Reputation context: The context in which a given reputation applies.
- $ Reputation proof: A non-interactive zero knowledge proof of a reputation signal.
- $ Reputation signal: A representative of a reputation.
- $ Service provider: An entity that provides a service on the Internet; examples services include hosted e-mail, e-commerce sites, and cloud computing platforms.

# IP address tracking

## IP address use cases

### Anti-abuse

IP addresses are a passive identifier used in defensive operations. They allow correlating requests, attribution, and recognizing numerous attacks, including:

- account takeover
- advertising fraud (e.g., click-fraud)
- disinformation operations (e.g., detecting scaled and/or coordinated attacks)
- financial fraud (e.g., stolen credit cards, email account compromise)
- malware/ransomware (e.g., detecting C2 connections)
- phishing
- real-world harm (e.g., child abuse)
- scraping (e.g., e-commerce, search)
- spam (e.g., email, comments)
- vulnerability exploitation (e.g., "hacking")

Malicious activity recognized by one service provider may be shared with other services {{!RFC5782}} as a way of limiting harm.

### DDoS and Botnets

Cyber-attackers can leverage the good reputation of an IP address to carry out specific attacks that wouldn't work otherwise. Main examples are Distributed Denial of Service (DDoS) attacks carried out by spoofing a trusted (i.e., having good reputation) IP address (which may or may not be the victim of the attack) so that the servers used to generate the DDoS traffic actually respond to the attackers trigger (i.e., spoofed packets). Similarly botnets may use spoofed addresses in order to gain access and attack services that otherwise would not be reachable.

### Multi-platform threat models

As siloed (single-platform) abuse defenses improve, abusers have moved to multi-platform threat models. For example, a public discussion platform with a culture of anonymity may redirect traffic to YouTube as a video library, bypassing YouTube defenses that otherwise reduce exposure of potentially harmful content. Similarly, a minor could be solicited by an adult impersonating a child on a popular social media platform, then redirected to a smaller, less established and less defended platform where illegal activity could occur. Phishing attacks are also common. There are many such cross-platform abuse models and they cause significant public harm. IP addresses are commonly used to investigate, understand and communicate these cross-platform threats. There are very few alternatives for cross-platform signals.

### Rough Geolocation

A rough geolocation can be inferred from a client's IP address, which is commonly known as either IP-Geo or Geo-IP. This information can have several useful implications. When abuse extends beyond attacks in the digital space, IP addresses may help identify the physical location of real-world harm, such as child exploitation.

#### Legal compliance

Legal and regulatory compliance often needs to take the jurisdiction of the client into account. This is especially important in cases where regulations are mutually contradictory (i.e. there is no way to be in legal compliance universally). Because Geo-IP is often bound to the IP addresses a given ISP uses, and ISPs tend to operate within national borders, Geo-IP tends to be a good fit for server operators to comply with local laws and regulations

#### Contractual obligations

Similar to legal compliance, some content and media has licensing terms that are valid only for certain locations. The rough geolocation derived from IP addresses allow this content to be hosted on the web.

#### Locally relevant content

Rough geolocation can also be useful to tailor content to the client's location simply to improve their experience. A search for "coffee shop" can include results of coffee shops within reasonable travel distance from a user rather than generic information about coffee shops, a merchant's website could show brick and mortar stores near the user and a news site can surface locally relevant news stories that wouldn't be as interesting to visitors from other locations.

## Implications of IP addresses

### Next-User Implications

When an attacker uses IP addresses with "good" reputations, the collateral damage poses a serious risk to legitimate service providers, developers, and end users. IP addresses may become assocaited with a "bad" reputation from temporal abuse, and legitimate users may be affected by blocklists as a result. This unintended impact may hurt the reputation of a service or an end user {{!RFC6269}}.

### Privacy Implications

IP addresses are sent in the clear throughout the packet journey over the Internet.
As such, any observer along the path can pick it up and use it for various tracking purposes. Beside basic information about the network or the device, it is possible to associate an IP address to an end user, hence, the relevance of IP addresses for user privacy. A very short list of information about user, device, and network that can be obtained via the IP address.

- Determine who owns and operates the network. Searching the WHOIS database using an IP address can provide a range of information about the organization to which the address is assigned, including a name, phone number, and civic address;
- Through a reverse DNS lookup and/or traceroute the computer name can be obtained, which often contains clues to logical and physical location;
- Geo-localisation of the device (hence the user) through various techniques {{GEOIP}}. Depending on the lookup tool used, this could include country, region/state, city, latitude/longitude, telephone area code and a location-specific map;
- Search the Internet using the IP address or computer names. The results of these searches might reveal peer-to-peer (P2P) activities (e.g., file sharing), records in web server log files, or glimpses of the individual's web activities (e.g., Wikipedia edits). These bits of individuals' online history may reveal their political inclinations, state of health, sexuality, religious sentiments and a range of other personal characteristics, preoccupations and individual interests;
- Seek information on any e-mail addresses used from a particular IP address which, in turn, could be the subject of further requests for subscriber information.

## IP Privacy Protection and Law

This section aim at providing some basic information about main example of laws adopted worldwide and related to IP address privacy (usually these laws area by product of the broader user privacy protection).

Possible content (to focus only on technical IP address related aspects):

- GDPR (General Data Protection Regulation) - EUROPE: Europe considers IP addresses as personal identification information that should be treated like any other personal information e.g. social security number.
- The United States has opted for a different approach to data protection. Instead of formulating one all-encompassing regulation such as the EU's GDPR, the US chose to implement sector-specific privacy and data protection regulations that work together with state laws to safeguard American citizens' data.
- In 2020, China released the first draft of Personal Information Protection Law (PIPL). The PIPL is the equivalent of European GDPR and will have significant influence.
- Japan Protection of Personal Information (APPI) Act (recent changes put the act close to the GDPR model).

## Mitigations for IP address tracking

The ability to track individual people by IP address has been well understood for decades. Commercial VPNs and Tor are the most common methods of mitigating IP address-based tracking.

- Commerical VPNs offer a layer of indirection between the user and the destination, however if the VPN endpoint's IP address is static then this simply substitutes one address for another. In addition, commerial VPNs replace tracking across sites with a single company that may track their users' activities.
- Tor is another mitigation option due to its dynamic path selection and distributed network of relays, however its current design suffers from degraded performance. In addition, correct application integration is difficult and not common.
- Address anonymization (e.g. {{GNATCATCHER}} and similar):
  - {{GNATCATCHER}} is a single-hop proxy system providing more protection against third-party tracking than a traditional commercial VPN. However, its design maintains the industry-standard reliance on IP addresses for anti-abuse purposes and it provides near backwards compatibility for select services that submit to periodic audits.
  - {{APPLEPRIV}} iCloud Private Relay is described as using two proxies between the client and server, and it would provide a level of protection somewhere between a commercial VPN and Tor.
- Recent interest has resulted in new protocols such as Oblivious DNS ([ODoH]({{I-D.pauly-oblivious-doh-02.html}})) and Oblivious HTTP ([OHTTP]({{I-D.thomson-http-oblivious}})). While they both prevent tracking by individual parties, they are not intended for the general-purpose web browsing use case.
- Temporary addresses

# Replacement signals for IP addresses

Fundamentally, the current ecosystem operates by making the immediate peer of a connection accountable for bad traffic, rather than the source of the traffic itself.  This is problematic because in some network architectures the peer node of the connection is simply routing traffic for other clients, and any client's use of that node may be only temporary.  Ideally, clients could present appropriate identification end-to-end that is separate from the IP address, and uniquely bound to a given connection.

## Evaluation of existing technologies

Technologies exist that are designed to solve some of the problems described in this document.

Privacy Pass {{?I-D.ietf-privacypass-protocol}} is a useful building block for solving numerous problems. Its design involves an interaction between a client and server where, at the end, the client is issued a set of anonymous tokens. These tokens may be redeemed at a later time, and this redemption should not be linkable with the initial issuance interaction. One existing use case is substituting a CAPTCHA challenge with a token, where successfully solving a CAPTCHA challenge results in a client being issued a set of anonymous tokens, and these tokens may be used in the future to bypass solving another CAPTCHA challenge. Therefore, Privacy Pass may be acceptable as an IS_HUMAN signal by some service providers. The current token design can't carry additional metadata like a user's reputation or an expiration date, and the tokens are not bound to an identity. The unlinkability property of the tokens is dependent on the implementation of key consistency {{?I-D.wood-key-consistency}}.

Trust Token {{TRUSTTOKEN}} is an extension of Privacy Pass where the issuance and redemption functionality are provided in the browser setting. The tokens are allowed to carry public and private metadata as extensions.

## Potential new technologies


# Security Considerations

TODO

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO
