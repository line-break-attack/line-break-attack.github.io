---
layout: default
---

<!-- Load Bootstrap CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css">

<style>
.btn-primary {
    background-color: #00b900 !important; 
    border-color: #00b900 !important;
}

.text-primary {
    color: #00b900 !important;
}

a {
    color: #00b900;
}
a:hover {
    color: #00b900;
}
</style>

<div align="center">
  <img src="logo.png" width="150">
</div>

# LINE-Break: Cryptanalysis and Reverse Engineering of Letter Sealing

LINE is a popular messaging platform used daily by millions of users in Southeast Asia -- most notably Japan, Taiwan, Thailand, and Indonesia.
It is particularly strong and an essential communication platform in Japan and Taiwan, with more than 85% of user adoption among the population.

We present a security analysis of the custom end-to-end encryption (E2EE) protocol underlying LINE, known as Letter Sealing v2 (LSv2).
Our findings show that Letter Sealing allows a TLS Man-in-the-Middle attacker or malicious server to violate integrity, authenticity, and confidentiality of communications in various experimentally verified attacks.

<a class="btn btn-primary" href="white-paper.pdf">Read the white paper</a>

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