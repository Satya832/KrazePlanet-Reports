## Title
Reflected XSS - Reddit
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://widgets.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The `post_id` path segment is reflected into an unquoted HTML `id` attribute without appropriate encoding. A crafted value can break out of the attribute and inject an image element whose `onerror` handler executes JavaScript. The proof of concept displays an alert containing `1`.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://widgets.pwnedbox.site/index.php/svc/shreddit/api/comments/askreddit/t3_u9po1l/t1_i5sxroa`.
2. Replace the `post_id` path segment (`t3_u9po1l`) with this exact value:
   ```text
   t3_u9po1ls@t><img src=x onerror=alert(1)>
   ```
3. Load the modified URL and observe an alert displaying `1`. The browser may show the spaces in the URL as `%20`.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Original comments page at the affected route:

<img width="1920" height="971" alt="1" src="https://github.com/user-attachments/assets/b3006a0c-5a80-4ac4-a0ab-a303279c7de6" />

2. Crafted path triggers the JavaScript alert: 

<img width="1920" height="971" alt="2" src="https://github.com/user-attachments/assets/56bb6bb1-937d-44e7-95f9-264a15b9f304" />

3. Page source shows the injected markup after the unquoted `id` value:

<img width="1920" height="966" alt="3" src="https://github.com/user-attachments/assets/5450a7bb-34c5-419f-899d-31eb127403ad" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
Opening a crafted link can execute JavaScript in the `widgets.pwnedbox.site` origin. Depending on the victim’s access and the site’s functionality, this could allow page manipulation or actions and data access available to same-origin JavaScript.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Apply HTML-attribute context encoding to `post_id` when rendering it, and quote the `id` attribute value.
2. Validate `post_id` against the application’s expected identifier format and reject invalid values.
