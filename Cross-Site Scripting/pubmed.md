## Title
Reflected XSS - PubMed: National Library of Medicine Search Builder
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pubmed.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality of the PubMed portal. The application accepts user input through the query parameter (term) and reflects it directly into the HTML response without proper sanitization or context-aware output encoding, allowing arbitrary JavaScript execution in the browser of any user who accesses a crafted link.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Navigate to the target application: [https://pubmed.pwnedbox.site/](https://pubmed.pwnedbox.site/)

2.Enter the XSS payload: <script>alert(1)</script> into the search or term query input field and submit.

3.Observe that the script executes and triggers an alert popup with the value 1, confirming the vulnerability via the term parameter ([https://pubmed.pwnedbox.site/index.php?term=](https://pubmed.pwnedbox.site/index.php?term=)<script>alert(1)</script>).

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
<img width="1920" height="969" alt="4" src="https://github.com/user-attachments/assets/1759074c-729f-49b3-b62d-e8cae3bb4c4f" />

## Impact
An attacker can craft malicious URLs containing script payloads and distribute them to users via phishing or external links. When a victim opens the crafted link, the malicious script executes within their active session, potentially leading to session hijacking, credential theft, or unauthorized actions performed on behalf of the user.

## Remediation
1.Implement context-aware output encoding for all user-supplied input rendered in the HTTP response (escaping special characters like <, >, &, ", and ').

2.Apply strict server-side validation and sanitization for the search query parameter (term).

3.Implement a robust Content Security Policy (CSP) to restrict inline script execution.
