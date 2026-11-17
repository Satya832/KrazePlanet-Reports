## Title
Reflected XSS - Function Name Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://profile.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability exists in the Last Name (lname) form field at [https://profile.pwnedbox.site/](https://profile.pwnedbox.site/), where user input is reflected in the response without proper sanitization. While the application attempts to block common functions like alert and confirm, it relies on an ineffective blacklist that misses vectors such as prompt() and case-insensitive tags like <sCript>, allowing attackers to execute arbitrary JavaScript in a victim's browser session.   

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Open [https://profile.pwnedbox.site/](https://profile.pwnedbox.site/).   

2.Enter "><sCript>prompt("Last")</sCript> into the Last Name field and Submit.
     
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.Payload: 

    "><sCript>prompt("Last")</sCript>

<img width="1920" height="968" alt="1" src="https://github.com/user-attachments/assets/af2082df-44e6-448d-b648-400e11e7e9f0" />

2.Execution of the JavaScript prompt dialogs upon page load:

<img width="1920" height="978" alt="2" src="https://github.com/user-attachments/assets/472d35b3-9f9f-468a-8f2d-e034d6973095" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
Successful exploitation of this reflected XSS vulnerability allows an attacker to craft a malicious link targeting [https://profile.pwnedbox.site/](https://profile.pwnedbox.site/) to steal session cookies, hijack user accounts, or execute unauthorized actions on behalf of the victim. Because the payload reflects directly in the browser context, attackers can also manipulate the DOM to inject phishing elements or redirect users to malicious domains.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Apply context-aware HTML encoding to all user-supplied input before reflecting it in the HTTP response so special characters are neutralized.

2.Discard brittle input filters that target specific terms like alert or confirm, as they can be easily bypassed using alternative vectors like prompt() or case variations.

Enforce Content Security Policy (CSP): Deploy a strong CSP header to restrict the execution of inline scripts and ensure scripts only load from trusted sources.
