## Title
Reflected XSS - Extended Function Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://account.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the lname GET parameter on the GET Parameter Reflection form. The parameter is reflected without proper HTML attribute-context encoding, allowing the input to break out of the form field’s value attribute and inject an <Img> element. Its invalid src triggers the onerror handler; the JavaScript Unicode escape \u0061 is interpreted as a, resulting in execution of alert(1).
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open https://account.pwnedbox.site/ and go to the GET Parameter Reflection form.
2. Leave First Name blank and enter s@t"> <Img src=x onerror=\u0061lert(1)> in Last Name.
3. Click Submit and observe the alert(1) popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The form is submitted with the payload in Last Name and First Name left blank.

<img width="1920" height="968" alt="1" src="https://github.com/user-attachments/assets/60a4ace7-a4cd-482a-b95b-0a8c4c1b4e2c" />

2. The reflected payload executes and displays the alert(1) popup.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/9ef7bde0-a881-43a5-9020-d6101d92ec31" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could craft a URL that executes JavaScript in a victim’s browser under the account.pwnedbox.site origin. Depending on the application and session protections, this could expose script-accessible page data or enable actions available to the victim.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-attribute encode lname wherever it is reflected—for example, in PHP use htmlspecialchars($lname, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8').
2. Do not render GET parameter values as raw HTML; remove reliance on tag or keyword blacklists and use context-aware output encoding.
