## Title
Reflected XSS - Shopify
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** `https://go.pwnedbox.site/?returnTo=`
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists on `go.pwnedbox.site` in the `returnTo` query parameter. When navigating through sections like **Account** (Confirm Account Details), the application takes the user-supplied value from the `returnTo` parameter and reflects it directly inside the `href` attribute of the **"Cancel & return"** `<a>` tag without proper HTML attribute encoding or URL validation. 

Because the input is rendered raw inside double quotes (`<a href="...">`), an attacker can break out of the `href` attribute context using `">` and inject arbitrary JavaScript, which executes immediately in the victim's browser when the page loads.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Navigate to `https://go.pwnedbox.site/` and click on the **Account** tab in the navigation bar.
2. Observe that the URL updates to `https://go.pwnedbox.site/?returnTo=https://accounts.shopify.com`.
3. Replace the `returnTo` parameter value with the following payload to break out of the `href` attribute and inject a script tag:
   ```text
   [https://go.pwnedbox.site/?returnTo=s@t](https://go.pwnedbox.site/?returnTo=s@t)"><script>alert(10)</script>
4. Load the URL in a browser and observe the alert(10) popup box executing immediately upon page load.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.  Discovering the returnTo parameter via the Account tab:

<img width="1920" height="968" alt="1" src="https://github.com/user-attachments/assets/827a1dd1-3c8f-44f4-83f2-188e3328c803" />

2.  Payload execution (alert(10)) in the browser:

<img width="1920" height="969" alt="2" src="https://github.com/user-attachments/assets/3207f18a-b784-4b8c-bbb0-d152f59b5170" />

3.  Page source confirming unencoded reflection inside the href attribute:

<img width="1919" height="971" alt="3" src="https://github.com/user-attachments/assets/1ee70725-d906-470e-98c2-98f6768982d3" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker who tricks a victim into opening a crafted link can execute arbitrary JavaScript in the victim's browser session on go.pwnedbox.site to access exposed account data or session tokens, perform unauthorized actions on the account page, or redirect the user to a phishing page.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.  HTML-encode returnTo with htmlspecialchars($returnTo, ENT_QUOTES, 'UTF-8') inside `<a href="...">`.   
2.  Allowlist valid redirect targets (such as relative paths or https://accounts.shopify.com) and reject untrusted URLs.
3.  Enforce safe URL schemes (https:// or relative paths) and block javascript: or data: URIs.
