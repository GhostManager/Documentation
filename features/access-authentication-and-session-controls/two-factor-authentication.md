---
description: Using Ghostwriter's support for TOTP 2FA
---

# Two-Factor Authentication

Ghostwriter v4+ supports the use of time-based, one-time passwords (TOTP) for two-factor authentication.

To enroll a device, visit your account profile and click the new _Set Up 2FA_ button. Ghostwriter will provide you with a QR code to scan with 1Password, Google Authenticator, or any other TOTP app you prefer. The setup page also provides a secret if you want to configure an app manually.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Enrolling in 2FA with a Password Vault App</p></figcaption></figure>

Administrators can configure accounts to require 2FA before the user can access Ghostwriter. Requiring 2FA is configurable on a per-user basis. With this option enabled, accounts without a valid TOTP device can only log in, log out, change their password, and access the 2FA setup page.

Ghostwriter’s 2FA also supports backup codes. You can generate and retrieve your backup codes from your profile page at any time after configuring 2FA.