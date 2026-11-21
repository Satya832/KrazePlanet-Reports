## Title
Reflected XSS to Account Takeover
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pixeleet.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The `search` parameter is vulnerable to reflected cross-site scripting (XSS). Submitting a script payload through the search field causes JavaScript to execute in the browser. The proof of concept displays `document.cookie`, and the resulting alert shows a JavaScript-accessible `PHPSESSID` cookie.The evidence confirms script execution and cookie exposure. It does not, by itself, verify that the cookie belongs to an authenticated session or that account takeover is possible.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Visit `https://pixeleet.pwnedbox.site/`.

2. Enter the following payload in the search field:

   ```html
   <script>alert(document.cookie)</script>
   ```

3. Submit the search and observe the JavaScript alert displaying the `PHPSESSID` cookie.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The search field containing the payload.

<img width="1920" height="969" alt="1" src="https://github.com/user-attachments/assets/435d0928-865d-443e-bae3-ed360e20dff3" />

2. The resulting alert displaying the `PHPSESSID` cookie.

<img width="1920" height="967" alt="2" src="https://github.com/user-attachments/assets/db197ac6-adec-4082-a809-59eaa5678d3e" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could send a crafted link to a victim and execute JavaScript in the context of `pixeleet.pwnedbox.site`. Because the proof of concept shows that `PHPSESSID` is accessible to JavaScript, an attacker may be able to steal and reuse it if it represents an active authenticated session. This could lead to session hijacking and potentially account takeover. Account takeover has not been independently verified.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Avoid inserting untrusted input as HTML; use safe text-rendering methods such as `textContent` where applicable.

2. Set session cookies with `HttpOnly`, `Secure`, and an appropriate `SameSite` value as defense in depth. These cookie attributes do not replace fixing the XSS.

3. Consider a Content Security Policy as an additional defense-in-depth measure.
