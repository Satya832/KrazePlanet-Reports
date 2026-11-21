## Title
Reflected XSS - Shopify
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** `https://go.pwnedbox.site/?returnTo=`
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the `returnTo` query parameter on `go.pwnedbox.site`. During the Account flow, the application reflects this user-controlled value directly into the quoted `href` attribute of the **“Cancel & return”** link without HTML attribute encoding. An attacker can use `">` to break out of the attribute and inject a `<script>` element, which executes when the page loads.
## Steps to Reproduce
1. Open `https://go.pwnedbox.site/` and click the **Account** tab to observe the `returnTo` parameter.

2. Replace the `returnTo` value with `s@t"><script>alert(10)</script>` and open this URL:  
   `https://go.pwnedbox.site/?returnTo=s@t"><script>alert(10)</script>`
3. Observe the `alert(10)` popup execute when the page loads.
   
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.  The Account tab reveals the `returnTo` parameter.  

<img width="1920" height="968" alt="1" src="https://github.com/user-attachments/assets/827a1dd1-3c8f-44f4-83f2-188e3328c803" />

2.  The injected payload executes and displays the `alert(10)` popup. 

<img width="1920" height="969" alt="2" src="https://github.com/user-attachments/assets/3207f18a-b784-4b8c-bbb0-d152f59b5170" />

3.  The page source shows the unencoded value reflected inside the `href` attribute.  

<img width="1919" height="971" alt="3" src="https://github.com/user-attachments/assets/1ee70725-d906-470e-98c2-98f6768982d3" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker who tricks a victim into opening a crafted link can execute JavaScript under the `go.pwnedbox.site` origin. Depending on the data and functionality exposed by that origin, this may allow access to script-readable page data or actions available to the victim.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-attribute encode `returnTo` before placing it in the `href` attribute. In PHP, use `htmlspecialchars($returnTo, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Parse and validate `returnTo` against an allowlist. Permit only relative paths or HTTPS URLs with an exact approved hostname; reject other hosts and schemes.
