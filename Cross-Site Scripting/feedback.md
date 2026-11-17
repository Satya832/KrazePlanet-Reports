## Title
Reflected XSS - Script Tag Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://feedback.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists within the fname and lname GET parameters of the feedback application at [https://feedback.pwnedbox.site/](https://feedback.pwnedbox.site/). The application reflects user-supplied input directly inside the input fields without proper output encoding. Utilizing mixed-case tags (<scRipt>) bypasses basic case-sensitive filters, allowing an attacker to break out of the attribute context using "> and execute arbitrary JavaScript.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Navigate to the target application at [https://feedback.pwnedbox.site/](https://feedback.pwnedbox.site/).   

2.Enter the payload "><scRipt>alert("First")</scriPt> into the First Name field and "><scRipt>alert("LAst")</scriPt> into the Last Name field and click Submit.   

3.Alternatively, open the crafted URL directly: [https://feedback.pwnedbox.site/?fname=](https://feedback.pwnedbox.site/?fname=)"><sCRipt>alert%28"First"%29%3C%2FSCRipt%3E&lname="><sCRipt>alert%28"LAst"%29%3C%2FSCRipt%3E.   

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.Payloads Used:

    "><scRipt>alert("First")</scriPt>
    
    "><scRipt>alert("LAst")</scriPt> 

<img width="1920" height="970" alt="1" src="https://github.com/user-attachments/assets/47fea875-bb69-4bc0-ac98-e1831d7b1dd9" />

2.The reflection of input inside form attributes and the resulting sequential alert executions.

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/314a21a1-382b-4fa7-8faa-d2d1be6e651c" />

<img width="1920" height="965" alt="3" src="https://github.com/user-attachments/assets/402fed40-3528-4a10-8806-5e69613e3e1e" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker can weaponize these payloads in phishing links to compromise authenticated users, leading to session hijacking, credential theft, or unauthorized actions performed on behalf of the victim.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Apply strict context-aware output encoding (converting special characters like <, >, ", and ' to HTML entities) before reflecting any user input.

2.Implement a strong Content Security Policy (CSP) to restrict inline script execution.
