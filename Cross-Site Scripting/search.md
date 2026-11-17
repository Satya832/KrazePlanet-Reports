## Title
Reflected XSS - Global Site Search Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://search.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists within the fname and lname parameters of the application. The application accepts user-supplied input and reflects it directly into the HTML response without proper context-aware output encoding. An attacker can inject arbitrary JavaScript payloads to execute in the browser of any user who visits a crafted URL.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Navigate to the target application: [https://search.pwnedbox.site/](https://search.pwnedbox.site/)

2.Enter the payload "><script>alert(1)</script> into the first form field (fname) and "><script>alert(2)</script> into the second form field (lname) and submit the form.

3.Observe that the payload breaks out of the input attribute context, leading to the execution of the injected scripts and triggering alert pop-ups ([https://search.pwnedbox.site/?fname=%22%3E%3Cscript%3Ealert%28%22First%22%29%3C%2Fscript%3E&lname=%22%3E%3Cscript%3Ealert%28%22Last%22%29%3C%2Fscript%3E](https://search.pwnedbox.site/?fname=%22%3E%3Cscript%3Ealert%28%22First%22%29%3C%2Fscript%3E&lname=%22%3E%3Cscript%3Ealert%28%22Last%22%29%3C%2Fscript%3E)).
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1.Payload Used: 

      "><script>alert("First")</script> 
   
      "><script>alert("Last")</script>

<img width="1920" height="976" alt="4" src="https://github.com/user-attachments/assets/3931139d-444f-4467-a1e1-afd28246f5e7" />


2.The reflection of input inside form attributes and the resulting sequential alert executions.

<img width="1917" height="966" alt="2" src="https://github.com/user-attachments/assets/6909a9d7-f358-45bc-b3fd-ed0325250aba" />

<img width="1917" height="967" alt="3" src="https://github.com/user-attachments/assets/61cd957b-f433-4a42-ad42-2abfa082c794" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
Successful exploitation allows an attacker to execute arbitrary JavaScript within a victim's browser session. This can lead to session hijacking, account takeover, or unauthorized actions performed on behalf of the user.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Implement strict HTML attribute encoding for all user-supplied data rendered inside HTML tags or attributes (such as converting double quotes `"` to `&quot;`, single quotes `'` to `&#x27;`, and ampersands `&` to `&amp;`), preventing attackers from breaking out of attribute boundaries.
   
2. Validate and sanitize all input accepted through `fname` and `lname` parameters to reject unexpected control characters or HTML syntax.

3. Deploy a strong CSP header that restricts inline script execution (`unsafe-inline`) to mitigate potential XSS impacts even if reflection occurs.
