---
title: "IP Address Privacy Considerations"
abbrev: "IP Address Privacy Use Cases"
docname: draft-ip-address-privacy-latest
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
    ins: S. Sahib
    name: Shivan Sahib
    organization: Brave Software
    email: shivankaulsahib@gmail.com
    


normative:

informative:




--- abstract

This document provides an overview of privacy considerations related to user IP addresses. It includes an analysis of some current use cases for tracking of user IP addresses, mainly in the context of anti-abuse. It discusses the privacy issues associated with such tracking and provides input on mechanisms to improve the privacy of this existing model. It then captures requirements for proposed 'replacement signals' for IP addresses from this analysis. In addition, existing and under-development techniques are evaluated for fulfilling these requirements.


--- middle

# Introduction

The initial intention of this draft is to capture an overview of the problem space and research on proposed solutions. The draft is likely to evolve significantly over time and may well split into multiple drafts as content is added.

Tracking of user IP addresses is common place on the Internet today, and is particularly widely used in the context of
anti-abuse, e.g. anti-fraud, DDoS management child protection activities. IP addresses are currently used as a source of
“reputation” in conjunction with other signals to protect against malicious traffic, since they are a relatively stable
identifier of the origin of a request. Servers use these reputations in determining whether or not a given packet, connection,
or flow corresponds to malicious traffic.

However, identifying the activity of users based on IP addresses has clear privacy implications e.g. user fingerprinting and cross site identity linking. Many technologies exist today to allow users to hide their IP address to avoid such tracking, e.g. VPNs, Tor. Several new technologies are also emerging in the landscape e.g. Gnatcatcher, Apple Private Relay and Oblivious techologies (OHTTPS, ODoH). 

This draft attempts to capture the following aspects of the tension between valid use cases for user identification and the related privacy concerns including:

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

Account abuse, financial fraud, ad fraud, child abuse...

### DDoS and Botnets

## Privacy implications of IP addresses

## Mitigations for IP address tracking

# Replacement signals for IP addresses
 
Fundamentally, the current ecosystem operates by making the paths of a connection accountable for bad traffic, rather than the
sources of the traffic itself. This is problematic because paths are shared by multiple clients and are impermanent. Ideally,
clients could present proof of reputation that is separate from the IP address, and uniquely bound to a given connection.

## Requirements

### Client requirements

- Clients MUST be able to request and present new reputation proofs on demand.
- A reputation signal MUST NOT be linkable to any identifying information for which the signal corresponds.
- Clients MUST be able to demonstrate good faith and improve reputation if needed.
- Clients MUST be able to dispute their reputation.
- Clients MUST be able to determine and verify the context in which a given reputation applies.
- Reputation signals MUST NOT remain valid indefinitely. (Clients must obtain new reputation signals periodically.)

### Server requirements

- Reputation signals MUST be bound to a context, and MUST NOT be transferrable across contexts.
- Clients MUST NOT be able to transfer reputations.

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
