## Title
Reflected XSS - Search Filter Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://kb.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search functionality on `kb.pwnedbox.site`. The user-controlled `search` parameter is included in the page without proper output encoding, so script markup supplied as the query executes in the browser. The results view shows an empty query label (`Results for ""`), while the injected script still runs, indicating the value is handled in an executable context rather than rendered as inert text.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://kb.pwnedbox.site/` and locate the search field.

2. Enter this exact payload in the search field:

   ```text
   <script>alert(1)</script>
   ```

3. Click **Search** and observe the `alert(1)` popup, with the payload reflected in the `search` parameter (browser-encoded in the address bar).
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. The payload `<script>alert(1)</script>` entered in the Knowledge Base search field.

<img width="1920" height="970" alt="1" src="https://github.com/user-attachments/assets/f5297fba-caa8-4915-a008-1f1a8810be52" />

2. Clicking **Search** executes the payload and displays the `alert(1)` popup.

<img width="1920" height="969" alt="2" src="https://github.com/user-attachments/assets/5c704cb4-a349-4aab-92f4-379d4673dca9" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `kb.pwnedbox.site` origin. Depending on the page’s functionality and the victim’s access, this could expose data available to page scripts, alter page content, or trigger actions through the victim’s session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-encode `search` wherever it is reflected. In PHP, use `htmlspecialchars($search, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Render the query as text rather than raw HTML; for client-side rendering, use `textContent` instead of `innerHTML`, and do not rely on tag stripping as a substitute for encoding.
