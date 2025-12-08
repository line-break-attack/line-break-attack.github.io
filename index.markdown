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

# LINE-Break: Cryptanalysis and Reverse Engineering of Letter Sealing

[LINE](https://www.line.me/en/) is a popular messaging platform used daily by millions of users in Southeast Asia -- most notably Japan, Taiwan, Thailand, and Indonesia.
It is particularly strong and an essential communication platform in Japan and Taiwan, with more than 85% of user adoption among the population.

We present a security analysis of the custom end-to-end encryption (E2EE) protocol underlying LINE, known as Letter Sealing v2 (LSv2).
Our findings show that Letter Sealing allows a TLS Man-in-the-Middle attacker or malicious server to violate integrity, authenticity, and confidentiality of communications in various experimentally verified attacks.

<div align="center">
  <a class="btn btn-primary" href="white-paper.pdf">Read the white paper</a>
</div>

## Overview

Letter Sealing is [claimed](https://scdn.line-apps.com/stf/linecorp/en/csr/line-encryption-whitepaper-ver2.1.pdf) to provide E2EE for text messages and media streams, ensuring that "no third parties or LINE Corporation can decrypt private calls and messages".

The company asserts confidentiality, partial forward security (between clients and servers only through TLS), integrity and authenticity.
LSv2 employs standardized primitives, such as ECDH for static key exchange using X25519, and AES256-GCM for payload encryption.

## Threat model

Our security analysis assumes several adversarial settings capturing both external and internal threats to an E2EE messenger. These models reflect realistic attack surfaces for LSv2:
* _Malicious Server_: The strongest adversary, controlling the LINE servers and capable of intercepting, modifying, replaying, or injecting any ciphertext exchanged between clients. This captures the case of a compromised or malicious service provider and is the primary threat model for E2EE, as the protocol should preserve confidentiality and authenticity even against an insider.
* _MitM Adversary_: A network attacker positioned between a client and the server, able to observe and manipulate traffic but without full server control. The MitM model is relevant when transport-layer protections (e.g., TLS) are disabled, bypassed, or compromised. Furthermore, an MitM attacker can emulate a malicious server by manipulating metadata.
* _Malicious User_: A legitimate participant in a one-to-one or group chat who misuses their possession of shared (group) keys to manipulate protocol state and break another user’s security properties (e.g., forging or replaying messages).

## Our results

We present the following attacks:
* _Equivocation attacks_: messages can be replayed, reordered, or dropped without the endpoints being aware, violating transcript consistency. This is inherent to the stateless nature of the protocol, which is insufficiently mitigated by easy-to-bypass server-side countermeasures. We show further attacks against integrity by tampering with read receipts.
* _Impersonation attacks_: the authorship of messages in one-to-one and group chats can be forged by a malicious user colluding with the adversary. This is a direct consequence of the way (group) keys are generated and managed in LSv2, and the lack of origin authentication measures. Combined with the previous attacks, we show how the adversary would theoretically be able to forge communications among a subset of parties in a group chat, or infiltrate a group chat and manipulate its following communication.
* _Plaintext leakage attacks_: there is substantial leakage of plaintext through usability features, such as stickers and URL previews. Even though the user is notified of the privacy-usability trade-offs around these features, we find that the leakage goes beyond what is publicly documented about the protocol.

The experiments were conducted using a Man-in-the-Middle (MitM) setup against an iOS client using a rogue root certificate, such that the adversary would be capable of tampering with protocol metadata in the same way as a malicious server.
We interact with LINE servers through both the official application and an independent JavaScript implementation of the client-side portion of the protocol, called [LINEJS](https://github.com/evex-dev/linejs).

## Disclosure

Our findings were disclosed in 06/06/2025 to the LY Corporation Computer Security Incident Response Team and later confirmed by the Letter Sealing Team, who provided a [statement](statement-ly-corporation.pdf) as a response.

## Team

We are a group of researchers from the [Department of Computer Science](https://www.cs.au.dk) at [Aarhus University](https://www.au.dk):
* Diego F. Aranha, Associate Professor
* Adam Blatchley Hansen, PhD student
* Thomas Kingo T. Mogensen, MSc.

</div>