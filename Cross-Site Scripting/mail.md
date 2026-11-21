## Title
Reflected XSS - Encoding Bypass Attempts
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://mail.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the `id` GET parameter on `mail.pwnedbox.site`. The parameter is reflected as executable markup rather than safely encoded text. An injected `<iMg>` element with an invalid `src` triggers its `onerror` handler. The handler uses the JavaScript Unicode escape `\u0061` for the `a` in `alert`, and the browser executes it.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Enumerate the GET parameters using:

   ```bash
   echo "https://mail.pwnedbox.site/" | x8scan
   ```

2. Open the following URL:

   ```text
   https://mail.pwnedbox.site/?id=s@t"><iMg src=x onerror=\u0061lert(2)>
   ```

3. Observe the JavaScript alert dialog.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. `x8scan` identifies the available GET parameters, including `id`.  

<img width="1908" height="1021" alt="1" src="https://github.com/user-attachments/assets/3c1d4208-937e-4793-8fe3-2e57816d1854" />

2. Testing the discovered parameters with mixed-case script-tag payloads.  

<img width="1920" height="972" alt="2" src="https://github.com/user-attachments/assets/8c9773d0-c5eb-4877-8f8a-03be4e848ff5" />

3. The image error-handler payload in `id` executes and displays an alert dialog

<img width="1920" height="971" alt="3" src="https://github.com/user-attachments/assets/e4d67bbf-ec9b-4606-a300-bba16a452107" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `mail.pwnedbox.site` origin. Depending on the page’s functionality and the victim’s access, this could allow page manipulation or access to data available to page scripts.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Apply context-appropriate output encoding to `id` before reflecting it in the HTML response. In PHP, for HTML text or quoted attributes, use `htmlspecialchars($id, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Treat the parameter as data rather than raw HTML, and do not rely on tag or keyword blacklists to prevent XSS.
