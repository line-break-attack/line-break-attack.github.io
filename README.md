<!-- Load Bootstrap CSS -->
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
>

<!-- (Optional) Bootstrap JS for components like modals -->
<script
  src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js">
</script>

<div align="center">
  <img src="logo.png" width="150">
</div>

# LINE-Break: Cryptanalysis and Reverse Engineering of Letter Sealing

LINE is a popular messaging platform used daily by millions of users in Southeast Asia -- most notably Japan, Taiwan, Thailand, and Indonesia.
It is particularly strong and an essential communication platform in Japan and Taiwan, with more than 85% of user adoption among the population.

We present a security analysis of the custom end-to-end encryption (E2EE) protocol underlying LINE, known as Letter Sealing v2.
Our findings show that Letter Sealing allows a TLS Man-in-the-Middle attacker or malicious server to violate integrity, authenticity, and confidentiality of communications in various experimentally verified attacks.

<a class="btn btn-primary" href="white-paper.pdf">Read the white paper</a>
