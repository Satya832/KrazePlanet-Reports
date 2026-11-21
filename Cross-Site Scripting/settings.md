## Title
Reflected XSS - Mixed Security Parameters
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://settings.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the `academy` GET parameter. The application applies a case-sensitive `str_replace` blacklist, then outputs the parameter directly using `echo $academy`. A mixed-case `<iMg>` tag bypasses the lowercase `img` filter, while `\u0061lert` bypasses the `alert` string filter and is interpreted by the browser as `alert`. The injected image’s `onerror` handler executes when the image fails to load.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Run the following command to enumerate GET parameters and identify `academy`:

   ```bash
   echo "https://settings.pwnedbox.site/" | x8scan
   ```

2. Open this URL with the payload in the `academy` parameter:

   ```text
   https://settings.pwnedbox.site/?academy=<iMg src=x onerror=\u0061lert(1)>
   ```

3. Observe the `alert(1)` popup.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. `x8scan` output identifies the `academy` GET parameter.  

<img width="1920" height="1021" alt="1" src="https://github.com/user-attachments/assets/a0a6d4c8-c1dd-4e74-a7f6-58e336baa6bf" />

2. The source code shows the case-sensitive blacklist and direct `echo $academy` output.  

<img width="1920" height="921" alt="2" src="https://github.com/user-attachments/assets/e93eec16-0019-4c79-b0bf-05b41fc32937" />

3. The crafted `academy` parameter triggers the `alert(1)` popup.  

<img width="1920" height="968" alt="3" src="https://github.com/user-attachments/assets/d179e9c7-4259-4dec-81c2-89ffbb24db07" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
A crafted link could execute JavaScript under the `settings.pwnedbox.site` origin. Depending on the page’s functionality and accessible data, this could allow page manipulation or access to data available to page scripts.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Encode `academy` before outputting it as HTML text. In PHP, use `htmlspecialchars($academy, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')` instead of `echo $academy`.

2. Remove the blacklist-based filtering; if the parameter has a restricted set of valid values, validate it against an allowlist.
