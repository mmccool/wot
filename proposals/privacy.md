# Normative Privacy Revisions
Following the meeting at TPAC 2019 in Fukuoka, in order to strengthen the privacy of systems based on WoT standards
the WoT WG agreed to explore adding normative privacy assertions to the WoT specifications
and to re-submit a CR including these additional assertions.
We are also considering a few other normative changes, specifically making certain items
optional so that they can be filtered out in privacy-sensitive contexts.

This document summarizes the proposed changes so that they can be reviewed in joint meetings between the WoT WG and the Privacy IG.

We will focus on the TD specification, as we are currently discussing making the Architecture document informative.
Therefore we propose consolidating the privacy and security considerations (and new normative statements) into
a single normative specification, the *Web of Things (WoT) Thing Description*.
Changes to the TD specification will include changes to existing assertions and normative content
and new assertions in a new normative "Privacy Mitigations" section.

## Privacy Risks
Privacy risks in the WoT fall into three broad categories: 
1. Data transmitted from a device (such as sensor data) that is associated with an identifiable person.
2. Personally identifiable information included in a Thing Description.
3. Information that can be inferred about a person from the information contained in a Thing description.

The WoT architecture is indended to reduce privacy risks due to Category 1 by encouraging an
open ecosystem (rather than a set of opaque proprietary systems) and making more visible the kinds of data that
a device is producing and sending to other devices.
Such visibility will allow devices to be better managed and the flows of such data controlled.
For example, a gateway could use rules to control whether certain types of information can leave the local network.
The tradeoff is the necessary *description* of the data produced by a device leads to increased risks in Categories 2 and 3.

TDs currently only have a few possible risks in Category 2 such as optional support emails, which we need to 
clarify should not be used for the email of the owner of the device.
It is also possible for TDs to be misused
and for developers to embed PII in free-form description fields (such as names of people, such as naming a device
"John's Insulin Monitor").  This should be explicitly forbidden.

In Category 3, PII can be inferred from the type of the device, which can be determined either directly
(through a link with a type-of relationship to a Thing Template), through semantic metadata explicitly giving the 
type of device, or by fingerprinting the set of affordances provided.
The first two mechanisms can be disallowed explicitly in privacy-sensitive contexts.
The last mechanism can be mitigated by filtering of affordances to a minimal set.

Risks in Categories 2 and 3 can also be mitigated by controlling the distribution of TDs and filtering them
(removing unnecessary information) when appropriate.

## Changes to Existing Assertions

### IDs to be made optional.
There are use cases where IDs are essential, i.e. device inventory management.
However,
in order to state assertions that require IDs to be omitted in some privacy-critical situtations,
they need to be made optional.
For example, an ad-hoc, one-time connection to a Thing may access a Thing Description only in order to
invoke a single action, and if the Consumer does not need to store the Thing Description or refer to it again
there is no need for an ID.
Even if an ID is used internally or in other contexts (eg when registering a device with a gateway)
it can be filtered out when providing the TD to consumers that do not need it.

Note: omitting the ID mitigates tracking risks but does not eliminate them.
In particular, it may be possible to infer the type of device by fingerprinting the set of affordances in
the TD and if the type of device is unusual, then it may be possible to use it for tracking.
This possibility can be reduced by further filtering the affordances provided in a TD for such ad-hoc
connections to a minimum number of affordances (or even just one).

#### Design Alternatives
An option here is to make the ID mandatory but use a workaround such as a temporary ID or an ID specific to each consumer.
However, if the intent is really to make the Thing anonymous simply omitting the ID is more straightforward.
Temporary IDs may still be necessary in some use cases, however.

## Mandatory Privacy Mitigations
These assertions will apply to any context where a device described by a TD may be associated with
a particular person.  We will call these "privacy sensitive contexts".

### TDs MUST not contain PII.
For example, a TD must not contain an owner's email or embed their name in a description or title field.

### TDs MUST be transmitted only to Authenticated and Authorized consumers.
Services providing TDs must require authentication (for example, using OAuth or PKI) of the requester to ensure they
are who they say they are.  In addition, the system needs to ensure that the requester is authorized to receive TDs.

### TDs provided to a Consumer MUST be filtered to remove any optional information not required by that Consumer.
For example, if a Consumer does not need human-readable titles or descriptions in a TD, these should be removed.
If a Consumer does not need to maintain state related to the consumed device, then an ID is not necessary and should be omitted.
If the Consumer does not do semantic processing, then semantic annotations can be omitted.

### Mechanisms to request TDs MUST include query parameters to specify the information provided.
The mechanism to request a TD should include query parameters to state the kinds of information necessary and the delivered
TD should be filtered to only include that information.

### TDs MUST be protected by encryption when at rest.
TDs must be stored in an encrypted data store.

### TDs MUST be protected by encryption when transmitted.
TDs must not be transmitted in plaintext.

### TDs MUST be stored for only a limited time appropriate to the application.

### TDs MUST NOT be forwarded to other parties without explicit approval of the owner.
Ideally TDs would not be forwarded at all and each Consumer would go directly to the source and
authenticate as appropriate.  However there are situations, such as local networks behind a NAT,
where TDs will need to be forwarded, i.e. by a proxy service.
In this case they can also be filtered to remove unnecessary information.