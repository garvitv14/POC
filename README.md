# Exploit Title: Google reCAPTCHA Bypass using X‑Skip‑Recaptcha HTTP header
# Exploit Author: Garvit Verma
# Vendor Name: **PHRASE** 
# Vendor Homepage: 
# Software Link:
# Version: v1.0
# Tested on: Kali Linux, Apache

Description
• A security flaw allows attackers to bypass reCAPTCHA validation entirely by including a custom HTTP header X-Skip-Recaptcha: true in requests. The server-side logic incorrectly trusts this header before verifying the Google reCAPTCHA token—thus granting access without actual human verification.
Vulnerability detail
• Endpoint logic performs reCAPTCHA validation unless it detects X-Skip-Recaptcha: true.
• Clients including this header are mistakenly allowed to pass bypass protections, effectively disabling reCAPTCHA enforcement.

Exploit POC:
curl -X POST https://your-app.com/form \
  -H "X-Skip-Recaptcha: true" \
  -d "name=Alice&...otherFormFields..."

This request bypasses reCAPTCHA and submits data without token verification.

Impact:
• Allows full reCAPTCHA bypass
• Enables spam, automated submissions, account creation automation, brute forcing, or abuse of protected functionality

Mitigation
• Remove any conditional checks for X-Skip-Recaptcha: true
• Enforce server-side reCAPTCHA token validation using Google’s API on every request
• Use secure header whitelisting; reject unknown or untrusted headers upstream (proxy/load‑balancer).
