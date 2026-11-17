## Title
Reflected XSS - PUBG
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://assets.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability was discovered in the search filter parameter (`p`) on the target application. The user-supplied input is reflected raw and directly inside a single-quoted HTML attribute (`data-query`) without proper sanitization or context-aware escaping (such as `htmlspecialchars`). This allows an attacker to break out of the attribute and execute arbitrary JavaScript code in the context of a victim's browser.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Navigate to `https://assets.pwnedbox.site/`
2.Locate the search filter input field in the community hub section.
3.Enter the following payload into the search box:
   ```html
   s@t'><script>alert(1)</script>
4.Click the Search button (or press Enter) to submit the GET request and Observe that the payload breaks out of the data-query attribute and triggers the JavaScript alert(1) popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.Payload Used:

        s@t'><script>alert(1)</script>

<img width="1920" height="970" alt="1" src="https://github.com/user-attachments/assets/578c207c-175f-4ec3-97e1-bdac16c18244" />


2.User input reflection within the search filter triggering script execution

<img width="1919" height="967" alt="2" src="https://github.com/user-attachments/assets/c5eace71-2164-48ed-b467-d4973cf33d64" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker can execute arbitrary JavaScript in a victim's browser session, enabling session hijacking, token theft, phishing, and unauthorized actions.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Ensure that all user-supplied data reflected in the application response is properly encoded before rendering.

2.For values reflected inside HTML attributes, use proper encoding functions like htmlspecialchars($p, ENT_QUOTES, 'UTF-8') to ensure single and double quotes are safely converted to entities.

3.Enforce strict CSP headers to mitigate the execution of unauthorized inline scripts and provide a robust defense-in-depth layer.
