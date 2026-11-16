## Title
Reflected XSS - RefSeek: Academic & Scientific Search Engine
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://refseek.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search bar feature of the RefSeek portal. The application accepts user input through the search query parameter (q) and reflects it directly into the HTML response without proper sanitization or context-aware output encoding, allowing arbitrary JavaScript execution in the browser of any user who clicks a crafted link.

## Steps to Reproduce
1.Navigate to the target application: [https://refseek.pwnedbox.site/](https://refseek.pwnedbox.site/)

2.Locate the search input box on the main page.

3.Enter the XSS payload: <script>alert(1)</script> into the search field and submit.

4.Observe that the script executes and triggers an alert popup with the value 1, confirming the vulnerability via the q parameter ([https://refseek.pwnedbox.site/index.php?q=](https://refseek.pwnedbox.site/index.php?q=)<script>alert(1)</script>).

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
<img width="1920" height="972" alt="3" src="https://github.com/user-attachments/assets/915c6b8b-d489-4c07-b9da-9a644e552b9a" />

## Impact
An attacker can craft malicious URLs containing script payloads and distribute them to users via phishing or external links. When a victim opens the crafted link, the malicious script executes within their active session, potentially leading to session hijacking, credential theft, or unauthorized actions performed on behalf of the user.

## Remediation
1.Implement context-aware output encoding for all user-supplied input rendered in the HTTP response (escaping special characters like <, >, &, ", and ').

2.Apply strict server-side validation and sanitization for the search query parameter (q).

3.Implement a robust Content Security Policy (CSP) to restrict inline script execution.
