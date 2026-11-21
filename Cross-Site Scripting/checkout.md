## Title
Reflected XSS - Multi-Parameter Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://checkout.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
Parameter enumeration identified several GET parameters, including `id`. Testing confirmed that the `id` value is reflected as executable HTML without proper output encoding. The payload breaks out of the reflected context and injects an image element; its invalid `src` triggers the `onerror` handler, where the Unicode escape `\u0061` resolves to `a` and executes `alert(2)`.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Enumerate the GET parameters with:

   ```bash
   echo "https://checkout.pwnedbox.site/" | x8scan
   ```

2. Set the `id` parameter to this payload and load the URL:

   ```text
   https://checkout.pwnedbox.site/?id=s@t"><iMg src=x onerror=\u0061lert(2)>
   ```

3. Observe the `alert(2)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. `x8scan` output identifying the available GET parameters.  

<img width="1920" height="1020" alt="1" src="https://github.com/user-attachments/assets/24734f6d-4d58-4bd8-8309-62d04599f647" />

2. Mixed-case `<scRipt>` payloads tested against multiple discovered parameters.  
 
<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/3adb9417-f85c-47cb-93e3-99be77e3fe2d" />

3. The `id` payload triggers the `alert(2)` popup.

<img width="1920" height="967" alt="3" src="https://github.com/user-attachments/assets/a668b471-7226-4aa4-8e8e-a629daaa8140" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `checkout.pwnedbox.site` origin. Depending on the page’s functionality and the victim’s access, this could allow page manipulation, access to data available to page scripts, or actions through the victim’s session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-attribute encode `id` wherever it is reflected. In PHP, use `htmlspecialchars($id, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Render the parameter as data, not raw HTML. If `id` has a defined format, validate it against that format as an additional check—not as a replacement for output encoding.
