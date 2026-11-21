## Title
Reflected XSS - TutorialRepublic: Web Development Reference Search
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://tutorialrepublic.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search bar feature of the TutorialRepublic website. The application takes user-supplied input via the search query parameter (q) and reflects it directly into the web page response without proper sanitization or output encoding, allowing arbitrary JavaScript execution in the context of a victim's browser.

## Steps to Reproduce
1. Open `https://tutorialrepublic.pwnedbox.site/` and locate the search bar.
2. Enter `<script>alert(1)</script>` in the search bar and submit it, or open this URL directly:

   ```text
   https://tutorialrepublic.pwnedbox.site/?q=<script>alert(1)</script>
   ```

3. Observe the `alert(1)` popup.

## POC (Proof of Concept)
1. The payload in the `q` parameter executes and displays the `alert(1)` popup.

<img width="1920" height="971" alt="1" src="https://github.com/user-attachments/assets/7c331119-a6d4-4cd1-b353-f961050ba642" />


## Impact
An attacker can craft a malicious link containing an XSS payload in the search parameter and trick users into clicking it via phishing or social media. When a victim opens the link, the script executes within their active session, potentially leading to session hijacking, credential theft, or unauthorized actions performed on the site on behalf of the user.

## Remediation
1. HTML-encode `q` wherever it is reflected in the response. In PHP, use `htmlspecialchars($q, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. If the search value is rendered client-side, insert it as text using `textContent` rather than treating it as HTML.

3.Deploy a Content Security Policy (CSP) to restrict unauthorized script execution.
