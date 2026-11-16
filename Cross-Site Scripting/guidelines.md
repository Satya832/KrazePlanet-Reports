## Title
Reflected XSS - GIGW: Government Website Guidelines Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://guidelines.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality of the portal. The application accepts user input through the search query parameter (s) and reflects it directly into the response page without proper sanitization or output encoding, allowing arbitrary JavaScript execution in the browser of any user who accesses a crafted link.

## Steps to Reproduce
1.Navigate to the target application: [https://guidelines.pwnedbox.site/](https://guidelines.pwnedbox.site/)

2.Locate the search input field in the header navigation or the search section.

3.Enter the XSS payload: <script>alert(1)</script> and submit the search.

4.Observe that the script executes and triggers an alert box with the value 1, confirming the vulnerability via the s parameter ([https://guidelines.pwnedbox.site/index.php?s=](https://guidelines.pwnedbox.site/index.php?s=)<script>alert(1)</script>).

## POC (Proof of Concept)
<img width="1920" height="970" alt="2" src="https://github.com/user-attachments/assets/22b018fd-c372-4fcd-b8e5-f06db64ada2f" />


## Impact
An attacker can craft malicious URLs containing script payloads and trick visitors into clicking them via phishing or external links. When clicked, the injected script executes within the victim's browser session, enabling attackers to hijack sessions, steal sensitive cookies, or perform actions on behalf of the user.

## Remediation
1.Implement context-aware output encoding for all user-supplied input before rendering it back in the HTML response (escaping special characters like <, >, &, ", and ').

2.Apply strict server-side input validation and sanitization for the search query parameter (s).

3.Deploy a robust Content Security Policy (CSP) to mitigate inline script execution risks.
