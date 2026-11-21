## Title
Reflected XSS - Function Name Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://profile.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability exists in the **Last Name** field. The application reflects the submitted value without proper HTML attribute-context encoding. Its function-name filter does not block `prompt()`, and the mixed-case `<scRipT>` tag is accepted, allowing JavaScript to execute in the browser.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Open `https://profile.pwnedbox.site/` and locate the **Last Name** field.

2. Enter this exact payload:

   ```text
   "><scRipT>prompt("Last")</scriPt>
   ```

3. Click **Submit** and observe the `prompt("Last")` dialog.
     
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Payload entered in the **Last Name** field.  

<img width="1920" height="968" alt="1" src="https://github.com/user-attachments/assets/af2082df-44e6-448d-b648-400e11e7e9f0" />

2. The reflected payload executes and displays the prompt dialog.  

<img width="1920" height="978" alt="2" src="https://github.com/user-attachments/assets/472d35b3-9f9f-468a-8f2d-e034d6973095" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
The payload executes JavaScript in the victim’s browser under the `profile.pwnedbox.site` origin. Depending on the page’s functionality and accessible data, this could allow page manipulation or actions to be performed through the victim’s browser session.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-attribute encode `lname` wherever it is reflected. In PHP, use `htmlspecialchars($lname, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')`.

2. Remove blacklist checks for specific function names or tag casing; render the user-supplied value as data, not raw HTML.

3.Deploy a strong CSP header to restrict the execution of inline scripts and ensure scripts only load from trusted sources.
