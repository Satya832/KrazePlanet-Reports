## Title
Reflected XSS - Category Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://shop.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) issue is demonstrated by supplying script markup in the catalog filter query parameters. The browser executes `alert(1)`, indicating that query-string input is reaching an executable context without appropriate handling. The captured request includes payloads in both parameters; test each parameter individually to confirm which one triggers execution.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://shop.pwnedbox.site/` and use the **Chemistry** category filter to observe the `category` and `sort` parameters in the URL.

2. Open this URL with the payloads in both parameters:

   ```text
   https://shop.pwnedbox.site/?category=<script>alert(1)</script>&sort=<script>alert(1)</script>
   ```

3. Observe the `alert(1)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The Chemistry filter link shows the `category` and `sort` query parameters.

<img width="1920" height="964" alt="1" src="https://github.com/user-attachments/assets/9f8ccabf-37d1-476e-b61d-e67d59b3cb80" />

2. The crafted URL triggers the `alert(1)` popup.

<img width="1920" height="974" alt="2" src="https://github.com/user-attachments/assets/8063985e-d727-4d71-9f50-91abe33ae70d" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `shop.pwnedbox.site` origin. Depending on the page’s functionality and the victim’s access, this could allow page manipulation or access to data available to page scripts.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Validate `category` and `sort` against the supported category and sort values, and reject unexpected values.

2. Contextually encode any reflected values before inserting them into the page; do not render query-parameter input as raw HTML.
