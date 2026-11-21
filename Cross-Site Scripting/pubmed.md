## Title
Reflected XSS - PubMed: National Library of Medicine Search Builder
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pubmed.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality on `pubmed.pwnedbox.site`. The application reflects the user-controlled `term` parameter into the HTML response without proper output encoding. As a result, script markup supplied as the search term executes when the crafted request is loaded.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://pubmed.pwnedbox.site/` and locate the search field.

2. Enter `<script>alert(1)</script>` as the search term and submit it, or open this URL directly:  

   `https://pubmed.pwnedbox.site/index.php?term=<script>alert(1)</script>`

4. Observe the `alert(1)` popup.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The `term` parameter payload executes and displays the `alert(1)` popup.

<img width="1920" height="969" alt="4" src="https://github.com/user-attachments/assets/1759074c-729f-49b3-b62d-e8cae3bb4c4f" />

## Impact
An attacker can craft malicious URLs containing script payloads and distribute them to users via phishing or external links. When a victim opens the crafted link, the malicious script executes within their active session, potentially leading to session hijacking, credential theft, or unauthorized actions performed on behalf of the user.

## Remediation
1. Encode `term` for its output context before reflecting it. For HTML text or quoted attributes in PHP, use `htmlspecialchars($term, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Render the search term as text rather than raw HTML; for client-side rendering, use `textContent` instead of `innerHTML`.
