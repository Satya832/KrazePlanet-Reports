## Title
Reflected XSS - Case-Insensitive Filter Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://directory.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists on directory.pwnedbox.site in the fname and lname GET parameters on the GET Parameter Reflection form. The server blocks <script> tags (including case-insensitive variants such as mixed-case <scRipt>), but it does not block <img> tags. Reflected input is written back unencoded, and a mixed-case <iMg> payload with an onerror handler executes JavaScript in the victim's browser context.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open https://directory.pwnedbox.site/ and go to the GET Parameter Reflection form.

2. Enter s@t"><iMg src=x onerror=confirm("First")> in First Name and s@t"><iMg src=x onerror=confirm("Last")> in Last Name.

3. Click Submit and observe confirm("Last") execute first, followed by confirm("First") after dismissing the popup.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The form submitted with the mixed-case <iMg> payloads in both name fields.

<img width="1920" height="968" alt="1" src="https://github.com/user-attachments/assets/02ad1383-fb57-4af1-b7e0-3e09993de5d8" />

2. The lname payload triggers the confirm("Last") popup.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/b840da38-ac9e-4c4d-84be-827b277231e8" />

3. After dismissing it, the fname payload triggers the confirm("First") popup.

<img width="1920" height="968" alt="3" src="https://github.com/user-attachments/assets/44cd954d-86cc-4578-9ca8-71db80084474" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted URL could cause JavaScript to execute in a victim’s browser under the site’s origin. Depending on the application and its protections, this could expose page data or allow actions available to the victim to be performed from their session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-attribute encode fname and lname before placing them in the form’s value attributes—for example, in PHP, use htmlspecialchars($value, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8').

2. Remove the <script>-only blacklist and never reflect these parameters as raw HTML; apply context-aware output encoding wherever they are rendered.
