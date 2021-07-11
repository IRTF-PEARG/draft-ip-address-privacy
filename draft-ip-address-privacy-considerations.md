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
    ins: T. BD
    name: Authors TBD
    organization: TBD
    email: tbd@tbd.com
    


normative:

informative:




--- abstract

This document provides an overview of privacy considerations related to user IP addresses. It includes an analysis of some current use cases for tracking of user IP addresses, mainly in the context of anti-abuse. It discusses the privacy issues associated with such tracking and provides input on mechanisms to improve the privacy of this existing model. It then captures requirements for proposed 'replacement signals' for IP addresses from this analysis. In addition, existing and under-development techniques are evaluated for fulfilling these requirements.


--- middle

# Introduction

The initial intention of this draft is to capture an overview of the problem space and research on proposed solutions. The draft is likely to evolve significantly over time and may well split into multiple drafts as content is added.

Tracking of user IP addresses is common place on the Internet today, and is particularly widely used in the context of
anti-abuse, e.g. anti-fraud, DDoS management, child protection activities. IP addresses are currently used as a source of
“reputation” in conjunction with other signals to protect against malicious traffic, since they are a relatively stable
identifier of the origin of a request. Servers use these reputations in determining whether or not a given packet, connection,
or flow corresponds to malicious traffic.

However, identifying the activity of users based on IP addresses has clear privacy implications e.g. user fingerprinting and cross site identity linking. Many technologies exist today to allow users to hide their IP address to avoid such tracking, e.g. VPNs, Tor. Several new technologies are also emerging in the landscape e.g. Gnatcatcher, Apple iCloud Private Relay and Oblivious techologies (OHTTPS, ODoH).

This draft attempts to capture the following aspects of the tension between valid use cases for user identification and the related privacy concerns including:

* An analysis of the current use cases, attempting to categorize/group such use cases where commonalities exist
* Find ways to enhance the privacy of existing uses of IP addresses.
* Generating requirements for proposed 'replacement signals' from this analysis (these could be different for each category/group of use cases)
* Research to evaluate existing technologies or propose new mechanisms for such signals

# Terminology

(Work in progress)

- Identity: Any identifying information about an end-user or service, be it a client or server, including IP addresses.
- Reputation: A random variable with some distribution. A reputation can either be "bad" or "good" with some probability according to the distribution.
- Reputation context: The context in which a given reputation applies.
- Reputation proof: A non-interactive zero knowledge proof of a reputation signal.
- Reputation signal: A representative of a reputation.

# IP address tracking

## IP address use cases

### Anti-abuse

Account abuse, financial fraud, ad fraud, child abuse...

### DDoS and Botnets

## Privacy implications of IP addresses

IP addresses provide a [relatively stable identifier](https://hal.inria.fr/hal-02435622), and are an important attribute in tracking people as they load web pages across sites. While the stable identifier is important in the above anti-abuse cases, this fact threatens a user's privacy because it allows for profiling of behavior. This profiling may occur anywhere on the path between the client and the server, inclusive. In addition, IP addresses passively leak meta information about the user, such as their rough geographical location. This may be beneficial, but not always as the default.

Some mitigations are discussed below, however any holistic solution must ensure privacy is available with no additional cost.

## Mitigations for IP address tracking

The ability to track individual people by IP address has been well understood for decades. Commercial VPNs and Tor are the most common methods of mitigating IP address-based tracking.

Commerical VPNs offer a layer of indirection between the user and the destination, however if the VPN endpoint's IP address is static then this simply substitutes one address for another. In addition, commerial VPNs replace tracking across sites with a single company that may track their users' activities.

Tor is another mitigation option due to its dynamic path selection and distributed network of relays, however its current design suffers from degraded performance. In addition, correct application integration is difficult and not common.

Recent interest has resulted in new protocols such as Oblivious DNS ([ODoH](https://www.ietf.org/staging/draft-pauly-oblivious-doh-02.html)) and Oblivious HTTP ([OHTTP](https://www.ietf.org/archive/id/draft-thomson-http-oblivious-00.html)). While they both prevent tracking by individual parties, they are not intended for the general-purpose web browsing use case.

Furthermore, [Gnatcatcher](https://github.com/bslassey/ip-blindness/blob/master/README.md) is a single-hop proxy system providing more protection against third-party tracking than a traditional commercial VPN. However, its design maintains the industry-standard reliance on IP addresses for anti-abuse purposes and it provides near backwards compatibility for select services that submit to periodic audits.

Finally, iCloud Private Relay is described as using two proxies between the client and server, and it would provide a level of protection somewhere between a commercial VPN and Tor.

# Replacement signals for IP addresses
 
Fundamentally, the current ecosystem operates by making the apparent path of a connection accountable for bad traffic, rather than the source of the traffic itself. This is problematic because in some cases IP addresses are shared by multiple clients (e.g., VPNs, Tor, carrier-grade NATs (CGNATs)) and any misbehavior associated with an address may be impermanent. Ideally, clients could gain a reputation and present proof of reputation that is separate from their apparent IP address, and uniquely bound
to a given connection.

Reputation services ([RFC7070](https://datatracker.ietf.org/doc/html/rfc7070)) are critical components present at multiple layers across the Internet and they are responsible for predicting whether a client will be abusive.  However, these services are constrainted by available identifiers when making a decision. As a result of this constraint, IP addresses tend to be an influential signal in the reputation assigned to an identity. Identifying alternatives for this dependency on IP addresses is
a goal of this document.

## Requirements

Some [considerations](https://datatracker.ietf.org/doc/html/draft-kucherawy-repute-consid-00) about reputation services are documented already from the perspective of organizations being operationally reliant on a third-party service. However, these considerations are relevant for and extend to a service's impact on clients, as well.

With the goal of replacing IP addresses as a fundemental signal in calculating a reputation, we describe two classes of requirements: properties of a replacement reputation signal, and properties of a reputation system. Each class is further divided into requirements of the client and requirements of the service.

### Required properties of replacement reputation signal

#### Client requirements

- A reputation signal MUST NOT be linkable to an Identity for which the signal corresponds.

#### Server requirements

- A reputation signal MUST NOT remain valid indefinitely meaning a client must obtain a new reputation signals periodically.
- A reputation signal MUST be bound to a reputation context, and MUST NOT be transferable across contexts.

### Requirements of a reputation system

#### Client requirements

- A client MUST be able to demonstrate good faith and improve reputation if needed.
- A client MUST be able to dispute their reputation.
- A client MUST be able to determine and verify the reputation context in which a given reputation applies.
- A client MUST be able to request and present new reputation proofs on demand.

#### Server requirements

- A reputation MUST be bound to an Identity, and MUST NOT be transferable.

## Evaluation of existing technologies

Technologies exist that solve problems in similar problem spaces, however none fulfill the above criteria.

[PrivacyPass](https://datatracker.ietf.org/doc/html/draft-ietf-privacypass-protocol-01) is not directly applicable for this use case, but it has been shown to be a useful building block for solving numerous problems. Its design simply allows substituting a CAPTCHA challenge with a token. The token can't carry additional information about the client's reputation, the token is not guaranteed to expire, and the tokens are not bound to an identity. Furthermore, PrivacyPass does not itself specify a reputation system, therefore it cannot be used to derive an unlinkable reputation signal.

[Trust Tokens](https://github.com/WICG/trust-token-api) are an extension of PrivacyPass where the tokens are allowed to carry private metadata. This additional metadata would allow for encoding information about a client's reputation, but Trust Tokens are not bound to an identity and they do not necessarily expire.

## Potential new technologies


# Security Considerations

TODO

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO
