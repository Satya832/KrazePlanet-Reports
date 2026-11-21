## Title
Reflected XSS - RefSeek: Academic & Scientific Search Engine
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://refseek.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality on `refseek.pwnedbox.site`. The application reflects the user-controlled `q` parameter in the HTML response without proper output encoding. As a result, script markup supplied in the search field executes in the browser when a crafted URL is loaded.

## Steps to Reproduce
1. Open `https://refseek.pwnedbox.site/` and locate the search input box.

2. Enter this payload into the search field and submit:

   ```text
   <script>alert(1)</script>
   ```

3. Observe the `alert(1)` popup, confirming execution via the `q` parameter in the URL.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The `q` parameter payload executes and displays the `alert(1)` popup.

<img width="1920" height="972" alt="3" src="https://github.com/user-attachments/assets/915c6b8b-d489-4c07-b9da-9a644e552b9a" />

## Impact
An attacker can craft malicious URLs containing script payloads and distribute them to users via phishing or external links. When a victim opens the crafted link, the malicious script executes within their active session, potentially leading to session hijacking, credential theft, or unauthorized actions performed on behalf of the user.

## Remediation
1. Encode `q` for its output context before reflecting it. For HTML text or quoted attributes in PHP, use `htmlspecialchars($q, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Render the search term as text rather than raw HTML; for client-side rendering, use `textContent` instead of `innerHTML`.

3.Implement a robust Content Security Policy (CSP) to restrict inline script execution.
