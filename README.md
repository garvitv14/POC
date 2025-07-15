reCAPTCHA Bypass via X-Skip-Recaptcha Header

Author: Garvit Verma

Tested On: Kali Linux, Apache

Version Affected: v1.0

Exploit Type: Server-side logic misconfiguration

Disclosure Status: Public

Summary
• A server-side implementation issue allows bypassing Google reCAPTCHA verification by including a custom HTTP header: X-Skip-Recaptcha: true. This header short-circuits CAPTCHA checks and allows requests to proceed without validation.
• While the application may return a 500 Internal Server Error for some actions (e.g., account creation), side effects like triggering verification emails still succeed—demonstrating that CAPTCHA checks were bypassed.

Technical Details
• The backend checks for a custom header X-Skip-Recaptcha.

• If present and set to true, the server skips reCAPTCHA validation.

• This behavior likely originates from a debugging or testing shortcut that was inadvertently left in production.

• No CAPTCHA token is required when this header is used.

Proof of Concept
curl -X POST https://target-app.com/form \
  -H "X-Skip-Recaptcha: true" \
  -d "name=Alice&email=alice@example.com&...otherFields..."

Impact
• reCAPTCHA can be bypassed on affected endpoints

• Automated submissions may be possible

• Verification emails or other actions could be triggered without solving CAPTCHA

• Potential abuse includes:

• Form spamming

• Account creation automation

• Brute-force attempts

• CAPTCHA rate-limiting circumvention
