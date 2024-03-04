---
title: Messaging Layer Security Ciphersuite using XWing Key Exchange Mechanism
abbrev: XWing ciphersuite for MLS
docname: draft-mahy-mls-xwing-latest
ipr: trust200902
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"area: art
workgroup: MLS
area: sec
category: info
keyword:
 - ML-KEM
 - Kyber
 - post-quantum
 - post quantum KEM
 - KEM
 - PQ/T
 - hybrid
 - hybrid KEM
 - Key Exchange Mechanism

stand_alone: yes
pi: [toc, sortrefs, symrefs]

venue:
  group: MLS
  type: Working Group
  mail: mls@ietf.org
  arch: https://mailarchive.ietf.org/arch/browse/mls/
  github: rohanmahy/mahy-mls-xwing/
#  latest: https://github.com/rohanmahy/mahy-mls-xwing/latest

author:
 -  ins: R. Mahy
    name: Rohan Mahy
    organization: Unaffiliated
    email: rohan.ietf@gmail.com

normative:

informative:
  MLKEM:
    target: https://csrc.nist.gov/pubs/fips/203/ipd
    title: 'FIPS 203 (Initial Draft): Module-Lattice-Based Key-Encapsulation Mechanism Standard'
    author:
      -
        ins: National Institute of Standards and Technology

--- abstract

This document registers a new Messaging Layer Security (MLS) ciphersuite using
the X-Wing hybrid post-quantum resistant / traditional (PQ/T) Key Exchange
Mechanism (KEM).

--- middle

# Introduction

The potential availability of a cryptographically-relevant quantum
computer has caused concern that well-funded adversaries could overturn
long-held assumptions about the security assurances of classical
Key Exchange Mechanisms (KEMs) and classical cryptographic signatures,
which are fundamental to modern security protocols, including the MLS
protocol {{!RFC9420}}.

The MLS Working Group has expressed strong desire to have a handful of
complimentary post-quantum security extensions for use with the MLS
protocol to address the related threats:

1. A straightforward MLS cipher suite that replaces a classical KEM with a
hybrid post-quantum/traditional KEM. Such a cipher suite could be
implemented as a drop-in replacement in many MLS libraries without changes
to any other part of the MLS stack. The aim is for implementations to have a
single KEM which would be performant and work for the vast majority of
implementations. It addresses the the harvest-now / decrypt-later threat model using the simplest, and most practicable solution available.
2. Versions of existing cipher suites that use post-quantum signatures; and
specific guidelines on the construction, use, and validation of hybrid
signatures.
3. One or more mechanisms which reduce the bandwidth or storage requirements,
or improve performance when using post-quantum algorithms (for example by updating post-quantum keys less frequently than classical keys, or by sharing portions of post-quantum keys across a large number of clients or groups.)

This document addresses the first of these work items. It reserves an MLS
cipher suite value based on the MLS default cipher suite, but replacing the KEM
with the X-Wing {{!I-D.connolly-cfrg-xwing-kem}} hybrid post-quantum /
traditional KEM. The IANA Hybrid Public Key Encryption (HPKE) {{!RFC9180}}
Key Exchange Mechanism (KEM) Identifier value for X-Wing is pending at the
time of this writing.



X-Wing is a "concrete, simple choice for [a] post-quantum hybrid KEM, that
should be suitable for the vast majority of use cases". X-Wing combines the
ML-KEM {{MLKEM}} post-quantum KEM and the X25519 {{?RFC7748}} traditional
KEM. The MLS cipher suite uses the other components of the default MLS cipher
suite `MLS_128_DHKEMX25519_AES128GCM_SHA256_Ed25519`.

This document replaces a previous, similar proposal based on the KEM in
{{!I-D.draft-westerbaan-cfrg-hpke-xyber768d00}}.

# Security Considerations

This ciphersuite uses a hybrid post-quantum/traditional KEM and a traditional
signature algorithm. As such, it is designed to provide confidentiality against
quantum and classical attacks, but provides authenticity against classical
attacks only. This is actually very useful, because an attacker could store
MLS-encrypted traffic that uses any classical KEM today. If years or decades in
the future a quantum attack on classical KEMs becomes feasible, the traffic sent
today (some of which could still be sensitive in the future) will then be readable.
By contrast, an attack on a signature algorithm in MLS would require an active
attack which can extract the private key during the signature key's lifetime.

The security properties of {{!I-D.connolly-cfrg-xwing-kem}} apply.

# IANA Considerations

This document registers a new MLS Ciphersuite value.

~~~
Value:        new assignment
Name:         MLS_128_XWING_AES128GCM_SHA256_Ed25519
Recommended:  N
Reference:    This document
~~~



--- back

# Acknowledgments
{:numbered="false"}

Thanks to Joël Alwen, Marta Mularczyk, Britta Hale, and Richard Barnes.
