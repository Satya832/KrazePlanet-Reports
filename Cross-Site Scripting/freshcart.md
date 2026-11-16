## Title
Reflected XSS - FreshCart: Online Grocery Store
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://freshcart.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality of the FreshCart grocery e-commerce portal. The application accepts user input through the search query parameter (q) and reflects it directly into the HTML response without proper sanitization or context-aware output encoding, allowing arbitrary JavaScript execution in the browser of any user who visits a crafted link.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Navigate to the target application: [https://freshcart.pwnedbox.site/](https://freshcart.pwnedbox.site/)

2.Locate the search input bar at the top of the homepage.

3.Enter the XSS payload: <script>alert(1)</script> into the search field and submit the query.

4.Observe that the script executes and triggers an alert popup containing the value 1, confirming the vulnerability via the q parameter ([https://freshcart.pwnedbox.site/index.php?q=](https://freshcart.pwnedbox.site/index.php?q=)<script>alert(1)</script>).
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
<img width="1920" height="969" alt="5" src="https://github.com/user-attachments/assets/bc572d02-d694-463b-9b1e-6462f28ac977" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker can craft malicious URLs containing script payloads and distribute them to users via phishing emails or social engineering. When a victim opens the crafted link, the malicious script executes within their active session, potentially leading to session hijacking, theft of sensitive cookies, credential harvesting, or unauthorized actions performed on behalf of the user.
## Remediation
1.Implement context-aware output encoding for all user-supplied input before rendering it back in the HTML response (converting special characters like <, >, &, ", and ' to their respective HTML entities).

2.Apply strict server-side validation and sanitization on the search query parameter (q).

3.Deploy a robust Content Security Policy (CSP) to restrict the execution of untrusted inline scripts.
