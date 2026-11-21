## Title
Reflected Cross-Site Scripting (XSS) via `returnTo` Parameter on `go.pwnedbox.site`

## Description
**Affected URL:** `https://go.pwnedbox.site/?returnTo=`  
**Vulnerable Parameter:** `returnTo` (GET)

A Reflected Cross-Site Scripting (XSS) vulnerability exists on `go.pwnedbox.site` in the `returnTo` query parameter. When navigating through sections like **Account** (Confirm Account Details), the application takes the user-supplied value from the `returnTo` parameter and reflects it directly inside the `href` attribute of the **Cancel & return** `<a>` tag without proper HTML attribute encoding or URL validation.

Because the input is rendered raw inside double quotes (`<a href="...">`), an attacker can break out of the `href` attribute context using `">` and inject arbitrary JavaScript, which executes immediately in the victim's browser when the page loads.

## Steps to Reproduce
1. Navigate to `https://go.pwnedbox.site/` and click on the **Account** tab in the navigation bar.
2. Observe that the URL updates to `https://go.pwnedbox.site/?returnTo=https://accounts.shopify.com`.
3. Replace the `returnTo` parameter value with `s@t"><script>alert(10)</script>` so the full URL becomes:

   `https://go.pwnedbox.site/?returnTo=s@t"><script>alert(10)</script>`

4. Load the URL in a browser and observe the `alert(10)` popup box executing immediately upon page load.

## POC (Proof of Concept)
1. Discovering the `returnTo` parameter via the **Account** tab:


2. Payload execution (`alert(10)`) in the browser:


3. Page source confirming unencoded reflection inside the `href` attribute:


## Impact
An attacker who tricks a victim into opening a crafted link can execute arbitrary JavaScript in the victim's browser session on `go.pwnedbox.site` to access exposed account data or session tokens, perform unauthorized actions on the account page, or redirect the user to a phishing page.

## Remediation
1. HTML-encode `returnTo` using `htmlspecialchars($returnTo, ENT_QUOTES, 'UTF-8')` inside `<a href="...">`.
2. Allowlist valid redirect targets (such as relative paths or `https://accounts.shopify.com`) and reject untrusted URLs.
3. Enforce safe URL schemes (`https://` or relative paths) and block `javascript:` or `data:` URIs.
