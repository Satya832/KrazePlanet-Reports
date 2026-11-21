## Title
Reflected XSS to Account Takeover
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pixeleet.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The search parameter is processed in a way that allows submitted HTML and JavaScript to execute on the search results page. The demonstrated payload runs `alert(document.cookie)`, and the alert displays a `PHPSESSID` value.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Visit `https://pixeleet.pwnedbox.site` and locate the search field in the page header.

2. Enter this exact payload in the search field:
   ```text
   <script>alert(document.cookie)</script>
   ```
3. Submit the search and observe an alert displaying the value of `document.cookie`, including a `PHPSESSID` cookie.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Search field with the payload:

<img width="1920" height="969" alt="1" src="https://github.com/user-attachments/assets/39597976-930e-4ede-9c10-71efd6dc67ba" />

2. Search results page showing the alert:

<img width="1920" height="967" alt="2" src="https://github.com/user-attachments/assets/746bad99-a608-4900-8937-8775b84a2976" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted search URL can execute JavaScript in the `pixeleet.pwnedbox.site` origin. As demonstrated, the script can read the `PHPSESSID` cookie. If the value belongs to an authenticated session, an attacker who obtains it may be able to impersonate that session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-encode the search term when rendering it in the response; do not insert it as raw HTML.

2. Set the session cookie with the `HttpOnly` attribute to prevent JavaScript access. This is defense-in-depth and does not replace fixing the XSS.
