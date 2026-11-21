## Title
Reflected XSS - Script Tag Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://feedback.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability affects the `fname` and `lname` GET parameters. The application reflects user-supplied values inside HTML input fields without proper attribute-context encoding. The payloads break out of the quoted attributes using `">`, while mixed-case script tags bypass the application’s case-sensitive filtering and execute JavaScript in the browser.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://feedback.pwnedbox.site/` and go to the GET Parameter Reflection form.

2. Enter these payloads in the corresponding fields:

   ```text
   First Name: "><scRipt>alert("First")</scriPt>
   Last Name: "><scRipt>alert("LAst")</scriPt>
   ```

3. Click **Submit** and observe both alert dialogs execute sequentially, dismissing each popup to see the next one.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The payloads entered in the **First Name** and **Last Name** fields.  

<img width="1920" height="970" alt="1" src="https://github.com/user-attachments/assets/47fea875-bb69-4bc0-ac98-e1831d7b1dd9" />

2. The `fname` payload executes and displays the `First` alert. 

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/314a21a1-382b-4fa7-8faa-d2d1be6e651c" />

3. The `lname` payload executes and displays the `LAst` alert.  

<img width="1920" height="965" alt="3" src="https://github.com/user-attachments/assets/402fed40-3528-4a10-8806-5e69613e3e1e" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker can weaponize these payloads in phishing links to compromise authenticated users, leading to session hijacking, credential theft, or unauthorized actions performed on behalf of the victim.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Apply strict context-aware output encoding (converting special characters like <, >, ", and ' to HTML entities) before reflecting any user input.

2.Implement a strong Content Security Policy (CSP) to restrict inline script execution.
