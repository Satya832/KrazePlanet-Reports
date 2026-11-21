## Title
Reflected XSS - Event Handler Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://tickets.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability affects the `lname` GET parameter on the GET Parameter Reflection form. The application reflects the value without proper HTML attribute-context encoding, allowing the input to break out of the field’s `value` attribute and inject an image element. Its invalid `src` triggers the `onError` handler; the JavaScript Unicode escape `\u0061` is interpreted as `a`, executing `alert(1)`.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://tickets.pwnedbox.site/` and go to the GET Parameter Reflection form.

2. Leave **First Name** blank and enter this payload in **Last Name**:

   ```text
   s@t"> <Img src=x onError=\u0061lert(1)>
   ```

3. Click **Submit** and observe the `alert(1)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The payload entered in **Last Name**, with **First Name** left blank.

<img width="1920" height="967" alt="1" src="https://github.com/user-attachments/assets/c95d75cb-9166-4497-9c07-c95989ba69bd" />

2. The reflected payload executes and displays the `alert(1)` popup. 

<img width="1920" height="972" alt="2" src="https://github.com/user-attachments/assets/60c20190-433c-42da-b82f-89e37e17306c" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could trick a victim into opening a crafted link that executes JavaScript under the `tickets.pwnedbox.site` origin. Depending on the site’s functionality and protections, this could allow page manipulation, access to script-readable data, or actions through the victim’s session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Apply HTML attribute-context encoding to `lname` before rendering it in the input’s `value` attribute. In PHP, use `htmlspecialchars($lname, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Do not rely on tag or function-name blacklists; render user-supplied values as data, not raw HTML.
