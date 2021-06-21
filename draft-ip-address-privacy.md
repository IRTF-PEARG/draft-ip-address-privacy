---
title: "Use Cases and Requirements for IP Address Privacy"
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

This document provides an analysis of the current use cases, privacy implications of and privacy mitigations for leakage of IP addresses. It examines the issues with anti-fraud reputation systems that rely on IP addresses currently and captures requirements for proposed 'replacement signals' from this analysis. In addition, existing and under-development techniques are evaluated for fulfilling these requirements.


--- middle

# Introduction

* An analysis of the current use cases, attempting to categorise/group such use cases where commonalities exist
* Generating requirements for proposed 'replacement signals' from this analysis (these could be different for each category/group of use cases)
* Research to evaluate existing technologies or propose new mechanisms for such signals
* Find ways to enhance the privacy of existing uses of IP addresses.



# IP address use cases

# Privacy implications of IP addresses

# Privacy mitigations for IP address leakage

# Replacements for IP addresses

## Reputation Systems

IP addresses are currently used as a source of “reputation” in conjunction with other signals to protect against malicious traffic. Servers use these reputations in determining whether or not a given packet, connection, or flow corresponds to malicious traffic. Tor exit nodes often have bad reputation because there exists some client sending bad traffic through the node. Absent any sort of signal beyond the IP address, all connections through the exit node are assigned the same reputation. Fundamentally, the current ecosystem operates by making the paths of a connection accountable for bad traffic, rather than the sources of the traffic itself. This is problematic because paths are shared by multiple clients and are impermanent. Ideally, clients could present proof of reputation that is separate from the IP address, and uniquely bound to a given connection. 

### Terminology

- Reputation: A random variable with some distribution. A reputation can either be "bad" or "good" with some probability according to the distribution.
- Reputation signal: A representative of a reputation.
- Reputation proof: A non-interactive zero knowledge proof of a reputation signal.
- Reputation context: The context in which a given reputation applies.
- Identity: Any identifying information about an end-user or service, be it a client or server, including IP addresses.

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

Original material from Matthew Finkel [on the PEARG mailing list](https://mailarchive.ietf.org/arch/msg/pearg/YbWZe6FtNMh9KkQrLMM3-dzbtjA/).


# Security Considerations

TODO

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO
