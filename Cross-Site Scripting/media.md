## Title
Reflected XSS - Imgur Mobile
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://media.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the `username` path parameter on the Messages page. The username is safely escaped when displayed as profile text, but is reflected without HTML attribute encoding into the **View profile** link’s `href` attribute. Supplying `">` breaks out of the quoted attribute and injects a script element, which executes when the page loads.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://media.pwnedbox.site/index.php/account/SaturnV/messages`.

2. Replace `SaturnV` in the path with the following payload and load the URL:

   ```text
   https://media.pwnedbox.site/index.php/account/s@t"><script>alert(1)</script>/messages#
   ```

3. Observe the `alert(1)` popup.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The Messages page displaying the profile and conversation list.

<img width="1920" height="969" alt="1" src="https://github.com/user-attachments/assets/755755a3-d79d-4b8d-85fb-2610ab495c32" />

2. The crafted username path triggers the `alert(1)` popup.

<img width="1920" height="967" alt="2" src="https://github.com/user-attachments/assets/d874cde7-ca42-4ed5-97d8-57031633ac5c" />

3. Page source shows the username escaped in the profile text but reflected unencoded in the profile link’s `href`.

<img width="1920" height="970" alt="3" src="https://github.com/user-attachments/assets/91ff8c19-e7f3-41b4-bfad-b01828302964" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could trick a victim into opening a crafted URL that executes JavaScript under the `media.pwnedbox.site` origin. Depending on the page’s functionality and the victim’s access, this could expose data available to page scripts, alter page content, or trigger actions through the victim’s session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. URL-encode the username as a single path segment, then HTML-attribute-encode the complete profile URL before placing it in `href`. In PHP, use `rawurlencode($username)` when building the path and `htmlspecialchars($profileUrl, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')` when rendering it.

2. Validate usernames against the application’s allowed format, but do not use validation as a replacement for output encoding.
