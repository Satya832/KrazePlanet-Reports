## Title
Reflected XSS - Page Heading
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://docs.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the `q` GET parameter on `docs.pwnedbox.site`. Although this parameter is not exposed in the visible form, it can be supplied directly in the request and is reflected into the page heading without HTML output encoding. The application filters `<script>` tags but accepts an `<iMg>` element with an `onerror` handler. The reflected markup is parsed as HTML, causing the handler to execute when the image fails to load.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://docs.pwnedbox.site/`.

2. Open the following URL with the payload supplied directly in the `q` parameter:

   ```text
   https://docs.pwnedbox.site/?q=s@t"><iMg src=x onerror=alert(1)>
   ```

3. Observe the `alert(1)` dialog.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Vulnerable site:

<img width="1920" height="973" alt="1" src="https://github.com/user-attachments/assets/14d0294c-81b0-4ad0-b0a0-9185438e67fb" />

2. Running `echo "https://docs.pwnedbox.site/" | x8scan` identifies the `q` parameter.  
      
<img width="1920" height="1021" alt="2" src="https://github.com/user-attachments/assets/0f03f7a3-c803-4ee8-8117-c53d161c5116" />

3. The payload in the `q` parameter triggers the `alert(1)` dialog.  

      `s@t"><iMg src=x onerror=alert(1)>`

<img width="1920" height="969" alt="3" src="https://github.com/user-attachments/assets/4fd3b1f0-0ca6-4568-a8b7-9e81add70740" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could trick a victim into opening a crafted link, causing JavaScript to execute under the `docs.pwnedbox.site` origin. This could allow the attacker to alter page content or access data available to scripts on the page.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-encode `q` before reflecting it in the page heading. In PHP, use `htmlspecialchars($q, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Render `q` as text rather than raw HTML. For client-side rendering, use `textContent` instead of `innerHTML`.
