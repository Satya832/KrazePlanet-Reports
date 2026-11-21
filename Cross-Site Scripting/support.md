## Title
Reflected XSS - Script & Img Tag Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://support.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists on `support.pwnedbox.site` in the `fname` and `lname` GET parameters on the GET Parameter Reflection form. The application uses a case-sensitive filter for `<script>` tags that can be bypassed using mixed casing (`<scRipt>` / `</scriPt>`) and reflects the input without proper encoding or attribute context protection, allowing arbitrary JavaScript execution in the browser.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://support.pwnedbox.site/` and go to the GET Parameter Reflection form.
  
2. Enter `s@t"> <scRipt>alert("First")</scriPt>` in **First Name** and `s@t"> <scRipt>alert("Last")</scriPt>` in **Last Name**.
   
3. Click **Submit** and observe `alert("First")` execute first, followed by `alert("Last")` after dismissing the popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Entering mixed-case `<scRipt>` payloads in First Name and Last Name and submitting the form:

<img width="1920" height="973" alt="1" src="https://github.com/user-attachments/assets/da9840ad-b198-403e-a2c2-95295751cce1" />

2. Execution of the `fname` payload (`First` alert popup):

<img width="1920" height="970" alt="2" src="https://github.com/user-attachments/assets/c9310662-9c78-45b6-b6ef-b4358f2d35dc" />

3. Execution of the `lname` payload (`Last` alert popup):

<img width="1920" height="968" alt="3" src="https://github.com/user-attachments/assets/53f229fe-bc9d-4846-9906-d081519d40c6" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker who tricks a victim into opening a crafted URL can execute arbitrary JavaScript in the victim's browser session on `support.pwnedbox.site` to access exposed page data or session tokens, perform unauthorized actions on behalf of the user, or redirect them to a malicious page.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Apply context-aware output encoding (such as `htmlspecialchars($input, ENT_QUOTES, 'UTF-8')`) before reflecting `fname` and `lname` in the response.
2. Replace case-sensitive keyword filtering with proper HTML sanitization and context-safe encoding rather than relying on string/tag blacklists.
