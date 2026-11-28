## Title
Stored XSS - Shopify
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://cms.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The article description accepts HTML that is saved and rendered using `innerHTML` on the **My Articles** page. An injected image error handler executes JavaScript when the saved article is displayed.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Visit https://cms.pwnedbox.site/ and sign in using the displayed test credentials:
   - **Email:** `liam@quill.io`
   - **Password:** `liam@123`
2. Click **Write** to create an article.
3. Click the `</>` button to edit the description's HTML source.
4. Enter the following payload in the **Description** field:
   ```html
   s@t"><iMg sRc=x onError=alert("Description")>
   ```
5. Enter a title and click **Save**.
6. Open **My Articles**. An alert displaying `Description` appears when the saved article is rendered.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The first screenshot shows the sign-in page and test credentials.

<img width="1920" height="971" alt="1" src="https://github.com/user-attachments/assets/8daf7336-f3bb-444b-9b4b-134ff1de5340" />

2. The second screenshot shows the article editor and HTML source button.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/e6568a2e-116d-4185-a63c-aff1f3530b1a" />

3. The third screenshot shows the payload entered in the article description.

<img width="1920" height="969" alt="3" src="https://github.com/user-attachments/assets/30c5d90c-9b7f-4f12-82d4-2dad8a8f5f90" />

4. The fourth screenshot shows the `Description` alert executing on the **My Articles** page.

<img width="1920" height="969" alt="4" src="https://github.com/user-attachments/assets/dc2162bc-5e87-4fad-86a1-c413807d9e00" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
JavaScript stored in an article description executes in the browser of a user who views the affected content. An attacker could use this to perform actions as that user or access data available to their session. The screenshots demonstrate execution in the test account; execution in another user's session is not shown.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Sanitize article HTML with a trusted allowlist-based HTML sanitizer before rendering it.
2. Remove event-handler attributes such as `onerror` and disallow unsafe URL schemes.
3. Use safe rendering APIs such as `textContent` where HTML formatting is not required.
4. Implement a restrictive Content Security Policy as defense in depth.
