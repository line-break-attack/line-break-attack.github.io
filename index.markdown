---
layout: default
---

<div align="center">
  <br>
  <img src="logo.png" width="150">
  <br>
  <br>
</div>

<div class="w-50 mx-auto">

# LINE-Break: Cryptanalysis of Letter Sealing

[LINE](https://www.line.me/en/) is a popular messaging platform in Southeast Asia, with over 85% adoption in Japan and Taiwan.

We analyze its underlying end-to-end encryption (E2EE) protocol Letter Sealing v2 (LSv2) and show that a TLS Man-in-the-Middle or malicious server can compromise integrity, authenticity, and confidentiality in multiple experimentally verified attacks.

<div align="center">
  <a class="btn btn-primary" href="white-paper.pdf">Read the white paper</a>
</div>

## Overview

Letter Sealing is [claimed](https://scdn.line-apps.com/stf/linecorp/en/csr/line-encryption-whitepaper-ver2.1.pdf) to provide E2EE for text messages and media streams, ensuring that "no third parties or LINE Corporation can decrypt private calls and messages".

The company asserts confidentiality, partial forward security (between clients and servers only through TLS), integrity and authenticity.
LSv2 employs standardized primitives, such as ECDH for static key exchange using X25519, and AES256-GCM for payload encryption.

## Threat model

We consider realistic adversaries:
* _Malicious Server_: Full control of LINE servers; can intercept, modify, replay, or inject messages. LSv2 should remain secure even here.
* _MitM Adversary_: Network attacker able to observe or modify traffic but without server control.
* _Malicious User_: Legitimate chat participant who misuses shared keys to forge or replay messages.

## Our results

Attacks include:
* _Equivocation_: Messages can be replayed, reordered, or dropped, violating consistency. Read receipts can also be tampered with.
* _Impersonation_: Message authorship can be forged by colluding users, exploiting LSv2 group key management and weak origin authentication.
* _Plaintext leakage_: Stickers, URL previews, and other usability features reveal more plaintext than documented.

Experiments used a MitM setup on iOS with a rogue root certificate. We tested both the official client and [LINEJS](https://github.com/evex-dev/linejs), a JavaScript implementation of the client-side protocol.

## Disclosure

Findings were reported on 06/06/2025 to LINE’s CSIRT and confirmed by the Letter Sealing Team, with an official [statement](statement-ly-corporation.pdf).

## Team

Researchers from [Aarhus University](https://www.au.dk):
* Diego F. Aranha, Associate Professor
* Adam Blatchley Hansen, PhD student
* Thomas Kingo T. Mogensen, MSc.

</div>
