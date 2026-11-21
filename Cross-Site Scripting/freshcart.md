## Title
Reflected XSS - FreshCart: Online Grocery Store
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://freshcart.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality on `freshcart.pwnedbox.site`. The application reflects the user-controlled `q` parameter in the HTML response without proper context-aware output encoding. As a result, script markup supplied in the parameter executes when a victim loads the crafted URL.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://freshcart.pwnedbox.site/` and locate the search bar.

2. Enter `<script>alert(1)</script>` in the search field and submit it, or open:  

   `https://freshcart.pwnedbox.site/index.php?q=<script>alert(1)</script>`

3. Observe the `alert(1)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The payload in the `q` parameter executes and displays the `alert(1)` popup. 
<img width="1920" height="969" alt="5" src="https://github.com/user-attachments/assets/bc572d02-d694-463b-9b1e-6462f28ac977" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker can craft malicious URLs containing script payloads and distribute them to users via phishing emails or social engineering. When a victim opens the crafted link, the malicious script executes within their active session, potentially leading to session hijacking, theft of sensitive cookies, credential harvesting, or unauthorized actions performed on behalf of the user.
## Remediation
1. Apply context-aware output encoding to `q` wherever it is reflected. For HTML text or quoted attributes in PHP, use `htmlspecialchars($q, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. If the value is rendered client-side, insert it as text (for example, with `textContent`) rather than treating it as HTML.
