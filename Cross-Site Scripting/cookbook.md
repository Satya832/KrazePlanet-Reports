## Title
Reflected XSS - Multi-Parameter Script Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://cookbook.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability affects the `fname` and `lname` GET parameters on `cookbook.pwnedbox.site`. User-supplied values are reflected inside HTML input `value` attributes without proper attribute-context encoding. The standard `<script>` payload is stripped in `fname`, but an `<img>` element with an `onerror` handler is accepted. In `lname`, a mixed-case `<scRipt>` payload bypasses the case-sensitive filter. Both payloads can break out of the attribute context and execute JavaScript.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://cookbook.pwnedbox.site/` and go to the GET Parameter Reflection form.

2. Enter these payloads in the corresponding fields:

   ```text
   First Name: s@t"><img src=x onerror=alert('First')>
   Last Name: s@t"><scRipt>alert("Last")</scriPt>
   ```

3. Click **Submit** and observe the `First` and `Last` alert dialogs.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.The form is populated with the payloads in the **First Name** and **Last Name** fields.  

    s@t"><img src=x onerror=alert('First')> 
  
    s@t"><scRipt>alert("Last")</scriPt>

<img width="1920" height="972" alt="1" src="https://github.com/user-attachments/assets/2d323b0a-e359-4f81-96af-50b9ce8fa49e" />

2.The `fname` payload triggers the `First` alert.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/dd475b22-661a-4bfb-a867-2922f05db018" />

3. The `lname` payload triggers the `Last` alert.

<img width="1920" height="968" alt="3" src="https://github.com/user-attachments/assets/81e47a3d-e794-419e-967a-41dc3c53ec03" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could trick a victim into opening a crafted link, causing JavaScript to execute under the `cookbook.pwnedbox.site` origin. Depending on the page and the victim’s access, this could expose data available to page scripts, alter page content, or trigger actions through the victim’s session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Implement strict context-aware HTML attribute encoding (converting characters like `"` to `&quot;`, `'` to `&#x27;`, and `<` to `&lt;`) for all reflected user input.

2.Avoid reliance on fragile blacklist filters or signature stripping for input validation.

3.Deploy a strict Content Security Policy (CSP) header restricting inline script execution (`unsafe-inline`).
