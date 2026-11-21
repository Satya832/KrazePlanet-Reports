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
1. Search field populated with the payload:

<img width="1920" height="969" alt="1" src="https://github.com/user-attachments/assets/39597976-930e-4ede-9c10-71efd6dc67ba" />

2. Search submission executes the payload and displays a cookie value in an alert:

<img width="1920" height="967" alt="2" src="https://github.com/user-attachments/assets/746bad99-a608-4900-8937-8775b84a2976" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
JavaScript executes in the `pixeleet.pwnedbox.site` origin and can access cookies available to `document.cookie`. The demonstrated alert displays a `PHPSESSID` value; if it identifies an authenticated session, exposure could enable session misuse, depending on server-side session protections.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Apply context-appropriate output encoding to the search value wherever it is rendered, and avoid inserting it as executable HTML.

2. Set session cookies with the `HttpOnly` attribute as defense-in-depth to prevent JavaScript from reading them.
