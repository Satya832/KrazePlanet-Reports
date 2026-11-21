## Title
Reflected XSS - Global Site Search Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://search.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability affects the `fname` and `lname` parameters. The application reflects their values inside HTML input attributes without proper attribute-context encoding. The payloads break out of those attributes and inject script elements, which execute when the response is loaded.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://search.pwnedbox.site/` and locate the form with the **First Name** and **Last Name** fields.

2. Enter these payloads in the corresponding fields:

   ```text
   First Name (fname): "><script>alert("First")</script>
   Last Name (lname): "><script>alert("Last")</script>
   ```

3. Click **Submit** and observe `alert("First")` execute, followed by `alert("Last")` after dismissing the first popup.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The form populated with the payloads in both name fields.

<img width="1920" height="976" alt="4" src="https://github.com/user-attachments/assets/3931139d-444f-4467-a1e1-afd28246f5e7" />

2. The `fname` payload executes and displays the `First` alert. 

<img width="1917" height="966" alt="2" src="https://github.com/user-attachments/assets/6909a9d7-f358-45bc-b3fd-ed0325250aba" />

3. The `lname` payload executes and displays the `Last` alert.

<img width="1917" height="967" alt="3" src="https://github.com/user-attachments/assets/61cd957b-f433-4a42-ad42-2abfa082c794" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could trick a victim into opening a crafted link that executes JavaScript under the `search.pwnedbox.site` origin. Depending on the page’s functionality and accessible data, this could expose data available to page scripts, alter page content, or trigger actions through the victim’s session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Implement strict HTML attribute encoding for all user-supplied data rendered inside HTML tags or attributes (such as converting double quotes `"` to `&quot;`, single quotes `'` to `&#x27;`, and ampersands `&` to `&amp;`), preventing attackers from breaking out of attribute boundaries.
   
2. Validate and sanitize all input accepted through `fname` and `lname` parameters to reject unexpected control characters or HTML syntax.

3. Deploy a strong CSP header that restricts inline script execution (`unsafe-inline`) to mitigate potential XSS impacts even if reflection occurs.
