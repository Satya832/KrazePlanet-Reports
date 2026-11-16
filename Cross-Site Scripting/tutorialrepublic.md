## Title
Reflected XSS - TutorialRepublic: Web Development Reference Search
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://tutorialrepublic.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search bar feature of the TutorialRepublic website. The application takes user-supplied input via the search query parameter (q) and reflects it directly into the web page response without proper sanitization or output encoding, allowing arbitrary JavaScript execution in the context of a victim's browser.

## Steps to Reproduce
1.Navigate to the target application: [https://tutorialrepublic.pwnedbox.site/](https://tutorialrepublic.pwnedbox.site/)

2.Locate the search input field in the top navigation bar.

3.Enter the XSS payload: <script>alert(1)</script> into the search bar and submit.

4.Observe that the script executes and triggers an alert pop-up with the value 1, confirming the reflection and execution of the injected script via the q parameter ([https://tutorialrepublic.pwnedbox.site/?q=](https://tutorialrepublic.pwnedbox.site/?q=)<script>alert(1)</script>).

## POC (Proof of Concept)
<img width="1920" height="971" alt="1" src="https://github.com/user-attachments/assets/7c331119-a6d4-4cd1-b353-f961050ba642" />


## Impact
An attacker can craft a malicious link containing an XSS payload in the search parameter and trick users into clicking it via phishing or social media. When a victim opens the link, the script executes within their active session, potentially leading to session hijacking, credential theft, or unauthorized actions performed on the site on behalf of the user.

## Remediation
1.Implement context-aware output encoding for all user input rendered in the HTTP response (converting special characters like <, >, &, ", and ' to HTML entities).

2.Validate and sanitize input parameters on the server side before reflection.

3.Deploy a Content Security Policy (CSP) to restrict unauthorized script execution.
