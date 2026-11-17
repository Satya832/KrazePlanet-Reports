## Title
Reflected XSS - Global Site Search Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://search.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the form input fields (fname and lname) of the KrazePlanetLabs application. The application processes user-supplied GET parameters and reflects them directly inside HTML input value attributes without proper sanitization or context-aware output encoding. By escaping the enclosing HTML attribute context (using "><script>), an attacker can inject and execute arbitrary JavaScript code within the browser of a victim who visits a crafted link.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Navigate to the target application: [https://search.pwnedbox.site/](https://search.pwnedbox.site/)

2.Locate the First Name (fname) and Last Name (lname) input fields within the form.

3.Input the payload "><script>alert("First")</script> into the First Name field and "><script>alert("Last")</script> into the Last Name field, then submit the form (or append them directly as GET parameters).

4.Observe that the payload breaks out of the input attribute context, leading to the execution of the injected scripts and triggering alert pop-ups ([https://search.pwnedbox.site/?fname=%22%3E%3Cscript%3Ealert%28%22First%22%29%3C%2Fscript%3E&lname=%22%3E%3Cscript%3Ealert%28%22Last%22%29%3C%2Fscript%3E](https://search.pwnedbox.site/?fname=%22%3E%3Cscript%3Ealert%28%22First%22%29%3C%2Fscript%3E&lname=%22%3E%3Cscript%3Ealert%28%22Last%22%29%3C%2Fscript%3E)).
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
Since the injection occurs directly within HTML form input value attributes (<input type="text" name="fname" value="[INJECTION]">), an attacker can craft malicious links pre-populating these fields with script payloads. If a victim clicks the link, the payload breaks out of the input box attribute and executes automatically in their browser session.This can lead to: 

1.Attribute Context Exploitation: Manipulating surrounding form elements or DOM structures.

2.Session Hijacking: Stealing sensitive session cookies or authentication tokens accessible via JavaScript.

3.Forced Actions: Performing unauthorized actions or requests on behalf of the authenticated user within the application context.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
## Remediation
1. Implement strict HTML attribute encoding for all user-supplied data rendered inside HTML tags or attributes (such as converting double quotes `"` to `&quot;`, single quotes `'` to `&#x27;`, and ampersands `&` to `&amp;`), preventing attackers from breaking out of attribute boundaries.
   
2. Validate and sanitize all input accepted through `fname` and `lname` parameters to reject unexpected control characters or HTML syntax.

3. Deploy a strong CSP header that restricts inline script execution (`unsafe-inline`) to mitigate potential XSS impacts even if reflection occurs.
