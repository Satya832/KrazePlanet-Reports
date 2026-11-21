## Title
Reflected XSS - HTML Tag Filter Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://news.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the hidden `title` GET parameter on `news.pwnedbox.site`. The parameter is reflected into the page’s HTML `<title>` element without proper output encoding. Supplying `</title>` closes that element, allowing an injected image tag with an `onerror` handler to execute JavaScript.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://news.pwnedbox.site/`.

2. Run the following command to identify the GET parameters, including the hidden `title` parameter:

   ```bash
   echo "https://news.pwnedbox.site/" | x8scan
   ```

3. Open the URL below and observe the `alert(1)` popup:

   ```text
   https://news.pwnedbox.site/?fname=&lname=&title=s@t</title><iMg src=x onerror=alert(1)>
   ```
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The target application’s GET Parameter Reflection form.

<img width="1920" height="972" alt="1" src="https://github.com/user-attachments/assets/0db7668a-77a7-4aa6-a926-32c06e9ee9a2" />

2. `x8scan` output identifying the `fname`, `lname`, and `title` parameters.

<img width="1920" height="1023" alt="2" src="https://github.com/user-attachments/assets/18605159-b9f2-4828-99e5-647e451c7e7c" />

3. The crafted `title` parameter triggers the `alert(1)` popup.

<img width="1919" height="969" alt="3" src="https://github.com/user-attachments/assets/ae5bb047-15bb-41d4-a22b-67aa287eed7c" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker could trick a victim into opening a crafted link that executes JavaScript under the `news.pwnedbox.site` origin. Depending on the page’s functionality and accessible data, this could allow page manipulation or access to data available to scripts.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-encode the `title` value before inserting it into the page’s `<title>` element. In PHP, use `htmlspecialchars($title, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Do not rely on tag blacklists; treat the parameter as text and encode it for its HTML output context.
