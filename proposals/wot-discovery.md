# WoT Discovery
abbreviation: wot-discovery

## Authors
* Michael McCool, Intel Corporation, michael.mccool@intel.com
* Carsten Bormann, Universität Bremen TZI, cabo@tzi.org
* Kunihiko Toumura (東村邦彦), Hitachi Ltd., kunihiko.toumura.yv@hitachi.com

## Abstract

W3C Web of Things (WoT) Thing Descriptions (TD) provide metadata that
describe the affordances and semantics of IoT devices and services.
However, before a TD can be interpreted and used, it needs to be found.
This document describes a discovery process for WoT TDs that also 
provides appropriate security and privacy access controls, supports 
multiple "first contact" mechanisms, supports advanced querying
capabilities, and supports both local and global discovery use cases.
This discovery process is based on CoRE Resource Directory {{CoRE-RD}}
with authentication following ACE {{ACE}} and can be accessed over either
CoAP {{CoAP}} or HTTP {{HTTP1.1}}.  To preserve privacy, the system
separates the discovery process into two stages, open
introduction protocols for first contact and a directory service that
provides querying capabilities and access to metadata, but only after
authentication and authorization.  Any introduction mechanism may be
used that can provide the address of a directory service but we will
specify several useful ones.  

# Introduction

This MAY {{?RFC2119}} be useful.

# Requirements

The WoT discovery process should have the following capabilities:
* Support peer-to-peer (self-identifying), local (network segment), and global (internet-wide) discovery
* Support various forms of query, including keyword, template, and semantic queries
* Be able to discover sleeping devices
* Support directory services (discovery via third-party services)
* Automatically clean up TDs for devices that are no longer accessible
* Support both static and dynamic TDs
* Support appropriate security and privacy goals
* Align with existing standards as much as possible
* Support WoT Scripting Discovery API

## Privacy-Preserving Architecture
* Support device and information lifecycle, trust management, access controls
* Distribute TDs only to authenticated and authorized users
* Don’t leak metadata or queries to unauthorized entities

## Alignment with existing standards
* Align with IETF CoRE Resource Directories, CoRE Link Format, and DID.
* Be accessible via a variety of existing discovery mechanisms,
  including DNS-SD, DNS-SRV, DHCP, QR codes, and Bluetooth beacons.

## References

### Normative
* [ACE-OAuth2: Authentication and Authorization for Constrained Environments (ACE) using the OAuth 2.0 Framework](https://datatracker.ietf.org/doc/draft-ietf-ace-oauth-authz/)
* [CoAP (RFC7252): The Constrained Application Protocol](https://datatracker.ietf.org/doc/rfc7252/)
* [CoRE-Link (RFC6690): Constrained RESTful Environments (CoRE) Link Format](https://datatracker.ietf.org/doc/rfc6690/)
* [CoRE-RD: CoRE Resource Directory](https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/)
* [HTTP1.1 (RFC2616): Hypertext Transfer Protocol -- HTTP/1.1](https://datatracker.ietf.org/doc/rfc2616/)
* [WoT-TD: W3C Web of Things (WoT) Thing Description](https://www.w3.org/TR/wot-thing-description/)
* [RFC2119: Key words for use in RFCs to Indicate Requirement Levels](https://datatracker.ietf.org/doc/rfc2119/)

### Informative
* [WoT-Arch: W3C Web of Things (WoT) Architecture](https://www.w3.org/TR/wot-architecture/)