## Title
Reflected XSS - GIGW: Government Website Guidelines Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://guidelines.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality on `guidelines.pwnedbox.site`. The application reflects the user-controlled `s` query parameter in the response without proper output encoding. As a result, script markup supplied as the search query executes in the browser when the crafted URL is loaded.

## Steps to Reproduce
1. Open `https://guidelines.pwnedbox.site/` and locate the search field.
  
2. Enter `<script>alert(1)</script>` in the search field and submit it. Alternatively, open this URL directly:

   `https://guidelines.pwnedbox.site/index.php?s=<script>alert(1)</script>`

3. Observe the `alert(1)` popup.

## POC (Proof of Concept)
1. The `s` parameter payload executes and displays the `alert(1)` popup. 
<img width="1920" height="970" alt="2" src="https://github.com/user-attachments/assets/22b018fd-c372-4fcd-b8e5-f06db64ada2f" />

## Impact
An attacker can craft malicious URLs containing script payloads and trick visitors into clicking them via phishing or external links. When clicked, the injected script executes within the victim's browser session, enabling attackers to hijack sessions, steal sensitive cookies, or perform actions on behalf of the user.

## Remediation
1. HTML-encode `s` wherever it is reflected in the response. In PHP, use `htmlspecialchars($s, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')` for HTML output.

2. If the query is rendered client-side, insert it as text using `textContent` rather than treating it as HTML.
