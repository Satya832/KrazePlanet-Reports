## Title
Reflected XSS - Page Heading
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://docs.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists within the application at docs.pwnedbox.site. Standard form fields (First Name and Last Name) do not reflect user input. However, enumeration using the x8scan tool revealed a hidden q GET parameter that reflects user-supplied input directly into the page heading. The application filters standard <script> tags, but accepts mixed-case or alternative vectors like an iMg tag with an onerror event handler. An attacker can use double quotes and closing brackets (">) to break out of the attribute/heading context and execute arbitrary JavaScript.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Run the x8scan tool against the target URL to discover hidden parameters: echo "[https://docs.pwnedbox.site/](https://docs.pwnedbox.site/)" | x8scan.   

2.Identify the discovered hidden parameter (q) which reflects input in the page heading.   

3.Navigate to the crafted vulnerable URL containing the payload: [https://docs.pwnedbox.site/?q=s%40t%22%3E%3CiMg%20src%3Dx%20onerror%3Dalert(1](https://docs.pwnedbox.site/?q=s%40t%22%3E%3CiMg%20src%3Dx%20onerror%3Dalert(1))>.   

4.Observe the execution of the JavaScript alert dialog.  
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.Vulnerable site:

<img width="1920" height="973" alt="1" src="https://github.com/user-attachments/assets/14d0294c-81b0-4ad0-b0a0-9185438e67fb" />


2.x8scan Results:

<img width="1920" height="1021" alt="2" src="https://github.com/user-attachments/assets/0f03f7a3-c803-4ee8-8117-c53d161c5116" />

3.Payload: 
s@t"><iMg src=x onerror=alert(1)>

<img width="1920" height="969" alt="3" src="https://github.com/user-attachments/assets/4fd3b1f0-0ca6-4568-a8b7-9e81add70740" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
Successful exploitation allows an attacker to execute arbitrary JavaScript within a victim's browser session via malicious links. This can lead to session token theft, account takeover, unauthorized actions performed on behalf of the user, and DOM-based phishing attacks.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Implement strict context-aware HTML output encoding for all reflected user input.

2.Avoid reliance on fragile blacklist filters or signature stripping for input validation.

3.Deploy a strict Content Security Policy (CSP) header restricting inline script execution (unsafe-inline).
