## Title
Reflected XSS - Equifax
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://tracking.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the `search` parameter on `tracking.pwnedbox.site`. A crafted search value is processed in an executable JavaScript context without adequate context-aware encoding. The payload invokes `alert(1)` through `[1].map(alert)`, and the alert executes when the crafted request is loaded.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://tracking.pwnedbox.site/` and locate the Help Center search field.

2. Enter this exact value in the search field:

   ```text
   ",{internalSearchTerm: [1].map(alert), numOfSearchResultsReturned: "2
   ```

3. Click **Search** and observe the `alert(1)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The crafted value entered in the search field.  

<img width="1920" height="972" alt="1" src="https://github.com/user-attachments/assets/b0512a7e-18c8-4d0a-8336-dbd8bf4ad004" />

2. Submitting the search triggers the `alert(1)` popup. 

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/d4b10be2-cef2-4e71-af0c-464579616c14" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `tracking.pwnedbox.site` origin. Depending on the page’s functionality and the victim’s access, this could expose data available to page scripts or allow page manipulation.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Safely serialize `search` for its JavaScript output context; do not concatenate it into executable JavaScript. In PHP, use `json_encode()` with appropriate `JSON_HEX_*` flags when embedding data in a script block.

2. If the query is only displayed in the page, render it as text using context-appropriate HTML encoding (or `textContent` for client-side rendering).
