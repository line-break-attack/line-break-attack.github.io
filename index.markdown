---
layout: default
---

<div align="center">
  <br><img src="logo.png" width="150"><br><br>
</div>

<div class="w-50 mx-auto">

# LINE-Break: Cryptanalysis and Reverse Engineering of Letter Sealing

[LINE](https://www.line.me/en/) is a popular messaging platform in Southeast Asia -- most notably Japan, Taiwan, Thailand, and Indonesia.
It is particularly strong and an essential communication tool in Japan and Taiwan, with more than 85% of user adoption among the population.

We analyze its underlying end-to-end encryption (E2EE) protocol Letter Sealing v2 (LSv2) and show that a TLS Man-in-the-Middle (MitM) or malicious server can compromise integrity, authenticity, and confidentiality in various experimentally verified attacks.

<div align="center">
    <a class="btn btn-primary" href="white-paper.pdf">Read the white paper</a>
</div>

## Overview

Letter Sealing claims to provide E2EE for messages and media, ensuring that "no third parties or LINE can decrypt private communications."  
It uses standardized primitives: X25519 for key exchange and AES256-GCM for encryption, supporting confidentiality, partial forward secrecy (TLS only), integrity, and authenticity.

## Threat model

We consider realistic adversaries:
* _Malicious Server_: Full control of LINE servers; can intercept, modify, replay, or inject messages. This captures the case of a compromised or malicious service provider and is the primary threat model for E2EE, LSv2 should remain secure even here.
* _MitM Adversary_: Network attacker able to observe or modify traffic but without server control. The MitM model is relevant when TLS is disabled, bypassed, or compromised. Furthermore, an MitM attacker can emulate a malicious server by manipulating metadata.
* _Malicious User_: Legitimate participant who misuses their possession of shared (group) keys to manipulate protocol state and break another user’s security properties.

## Our results

We present the following attacks:
* __Equivocation attacks__: messages can be replayed, reordered, or dropped without the endpoints being aware, violating transcript consistency. This is inherent to the stateless nature of the protocol, which is insufficiently mitigated by easy-to-bypass server-side countermeasures.
* __Impersonation attacks__: the authorship of messages in one-to-one and group chats can be forged by a malicious user colluding with the adversary. This is a direct consequence of the way (group) keys are generated and managed in LSv2, and the lack of origin authentication measures.
* __Plaintext leakage attacks__: there is substantial leakage of plaintext through usability features, such as stickers and URL previews. We observe that these features reveal more plaintext than what is publicly documented about the protocol..

Combining these attacks, we show how the adversary would theoretically be able to forge communications among a subset of parties in a group chat, or infiltrate a group chat and manipulate its following communication.

The experiments were conducted using a MitM setup against an iOS client using a rogue root certificate, such that the adversary would be capable of tampering with protocol metadata in the same way as a malicious server.
We interact with LINE servers through both the official application and an independent JavaScript implementation of the client-side portion of the protocol, called [LINEJS](https://github.com/evex-dev/linejs).

## Disclosure

Our findings were disclosed in 06/06/2025 to the LY Corporation Computer Security Incident Response Team and later confirmed by the Letter Sealing Team, who provided a [statement](statement-ly-corporation.pdf) as a response.

## Team

We are a group of researchers from the [Department of Computer Science](https://www.cs.au.dk) at [Aarhus University](https://www.au.dk):
* Diego F. Aranha, Associate Professor
* Adam Blatchley Hansen, PhD student
* Thomas Kingo T. Mogensen, MSc.

</div>