## Title
Reflected XSS - Search Filter Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://kb.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability affects the catalog’s `category` and `sort` query parameters. The crafted request supplies script markup through both parameters, and the page executes the injected JavaScript, demonstrating that the reflected input is not safely handled.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://shop.pwnedbox.site/` and interact with a category filter to observe the `category` and `sort` parameters in the URL.

2. Open this URL with the payloads in both parameters:

   ```text
   https://shop.pwnedbox.site/?category=<script>alert(1)</script>&sort=<script>alert(1)</script>
   ```

3. Observe the `alert(1)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The category filter reveals the `category` and `sort` query parameters.  

<img width="1920" height="964" alt="1" src="https://github.com/user-attachments/assets/ae9d3763-4763-4db8-a343-189148934502" />

2. The crafted URL triggers the `alert(1)` popup.  

<img width="1920" height="974" alt="2" src="https://github.com/user-attachments/assets/bf04132e-d884-4876-a176-7800d92c0790" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `shop.pwnedbox.site` origin. Depending on the page’s functionality and the victim’s access, this could allow page manipulation or access to data available to page scripts.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Validate `category` and `sort` against the supported filter and sort values, and reject unexpected values.

2. Contextually encode both parameters wherever they are reflected in the HTML response; do not render them as raw HTML.
