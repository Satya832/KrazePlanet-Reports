## Title
Reflected XSS - PUBG
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://assets.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists on the application via the p parameter. The input supplied in the search/filter functionality is unsanitized and reflected directly inside a HTML attribute (data-query='...') without proper encoding or HTML entity conversion. This allows an attacker to inject and execute arbitrary JavaScript code in the context of a victim's browser session.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://assets.pwnedbox.site/` and locate the search or filter field.

2. Enter this payload into the field:

   ```text
   s@t'><script>alert(1)</script>
   ```

3. Click **Search** or press **Enter**, then observe the `alert(1)` popup.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Payload entered in the search field.
   
       s@t'><script>alert(1)</script>

<img width="1920" height="970" alt="1" src="https://github.com/user-attachments/assets/21e8f06f-3b26-4421-9308-518c9461483b" />

2. The injected payload executes and displays the `alert(1)` popup.  

<img width="1919" height="967" alt="2" src="https://github.com/user-attachments/assets/6c602aaa-3bb4-48e3-9385-3e827fcf6688" />

3. Page source showing the vulnerable reflection in the `data-query` attribute. 

<img width="1920" height="967" alt="3" src="https://github.com/user-attachments/assets/f5726f00-1c3b-4f9e-ad2d-dadf0d1addae" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker can exploit this vulnerability to execute malicious JavaScript within the context of a victim's browser session. This can lead to session hijacking, stealing sensitive user cookies or tokens, performing unauthorized actions on behalf of the user, or redirecting the user to malicious external sites.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Implement proper context-aware output encoding. Since the user input is reflected inside an HTML attribute, all user-supplied data must be properly HTML-entity encoded (e.g., converting ' to `&#39;`, < to `&lt;`, and > to `&gt;`) before rendering it in the response.

2. Utilize a robust web framework or template engine that automatically handles contextual escaping.

3. Implement a strong Content Security Policy (CSP) to restrict the execution of inline scripts.
