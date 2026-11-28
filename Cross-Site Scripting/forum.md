## Title
Stored XSS - Acronis
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://forum.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The profile signature is rendered as HTML without adequately sanitizing user input. An injected image error handler executes JavaScript in the profile's live preview. The profile save confirmation also states that the signature is active on all posts. The screenshots show execution of the signature payload. They do not demonstrate execution from the **Display Name** or **About Me** fields, or in another user's browser.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Visit https://forum.pwnedbox.site/ and register a test account.
2. Sign in and select **My Profile**.
3. Enter the following payload in the **Signature** field:
   ```html
   <img src=x onerror=confirm("Signature")>
   ```
4. Click **Save Profile**.
5. Observe the confirmation dialog displaying `Signature` when the signature is rendered.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The first screenshot shows the test account registration form.

<img width="1920" height="967" alt="1" src="https://github.com/user-attachments/assets/f8fc4454-380a-4a5f-911d-340f23d64eda" />

2. The second screenshot shows test payloads entered in the profile fields, including the signature.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/0c82ce14-a32b-4830-9ffd-d38cb21012c2" />

3. The third screenshot shows the `Signature` confirmation dialog and the profile save confirmation.

<img width="1920" height="965" alt="3" src="https://github.com/user-attachments/assets/6e064872-f860-4e18-bc68-58d8ba47abe4" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could store JavaScript in their profile signature. If the signature is rendered for other users without sanitization, the script could execute in their browsers and perform actions or access data available to their sessions.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Sanitize signature HTML with a trusted allowlist-based HTML sanitizer before rendering it.
2. Remove event-handler attributes such as `onerror` and disallow unsafe URL schemes.
3. Use `textContent` when HTML formatting is not required.
4. Apply the same protection to the live preview and every page that displays signatures.
5. Implement a restrictive Content Security Policy as defense in depth.
