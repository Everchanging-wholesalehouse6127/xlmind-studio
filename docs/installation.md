# Installation

How to install and activate **XLMind Studio**, the Windows desktop add-in for Microsoft Excel.

## Requirements

| | |
|---|---|
| **Operating system** | Windows desktop |
| **Microsoft Excel** | 64-bit **desktop** Excel on Windows |
| **Architecture** | 64-bit only |
| **Permissions** | Administrator permission may be required, depending on Windows and company policies |
| **Internet** | Not required for the tools themselves; may be required for licensing and activation, payment, download, update checks, in-product information messages and support |

**Not supported:** 32-bit Office installations, Excel for macOS, Excel for the web, mobile Excel apps, and Office installations outside the verified support matrix.

> The exact supported Windows and Excel versions, required runtime components, disk space and installation permissions are published on the official [system requirements page](https://xlmindstudio.com/en/system-requirements?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=installation_sysreq) after verification against the current installer and build configuration. That page — not this one — is the authoritative compatibility matrix, and it is deliberately not duplicated here so that it cannot go out of date.

### Which Excel do I have?

In Excel, go to **File › Account › About Excel**. The first line of the dialog shows the version and, at the end, either **32-bit** or **64-bit**. XLMind Studio requires the 64-bit build.

## Download

Download the installer from the official download page and nowhere else:

→ **<https://xlmindstudio.com/en/download?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=installation_download>**

XLMind Studio installers are **not** published as GitHub Releases and are not distributed through mirrors, download portals or file-sharing sites. A build obtained anywhere else cannot be verified.

## Verify before you install (recommended)

Every release is digitally signed and published with a SHA-256 checksum. Two minutes of checking is worth it, especially on a corporate device:

1. Right-click the installer › **Properties** › **Digital Signatures** — confirm the publisher shown there matches the publisher listed on the [security page](https://xlmindstudio.com/en/security).
2. Compare the SHA-256 checksum of your download with the value published on the same page.

Step-by-step, including the PowerShell command: [security.md](security.md).

## Install

1. **Close Microsoft Excel.** The add-in registers with Excel during installation.
2. Run the installer and follow the prompts.
3. Approve the administrator prompt if Windows asks for it. On a managed corporate device, your IT team may need to approve the installation.
4. If Windows SmartScreen shows a notice, see [troubleshooting.md](troubleshooting.md) before continuing — it explains what the notice means and how to confirm the publisher first.

## First run

1. Open Microsoft Excel.
2. Look for the **XLMind Data** and **XLMind Plus** tabs on the ribbon.
3. Open the **Account** group on either tab.

If the tabs do not appear, work through [troubleshooting.md](troubleshooting.md) — the usual cause is that the add-in was disabled by Excel or that Excel is a 32-bit installation.

## Trial and activation

**Free trial** — 7 days, all features available. The period starts with the first successful trial activation, and it does not convert into a paid license automatically.

**Activating a purchased license**

1. On the **XLMind Data** or **XLMind Plus** ribbon tab, open the **Account** group.
2. Select **Activate**.
3. Enter the license code that was emailed to you after purchase.

License codes are delivered only by email, to the address used for the purchase — they are not shown on the payment confirmation page. If the email has not arrived, check your spam folder, then contact <support@xlmindstudio.com> with your order number.

**Moving to a new computer** — activate on the new machine with the same license code. If the device limit is already full, the oldest active device is deactivated automatically.

## Updates

- Check for updates from inside the product, or use the update link provided to you.
- Back up important workbooks before installing an update.
- Update eligibility and duration are described in the [Update & Support Policy](https://xlmindstudio.com/en/updates-and-support).

## Uninstalling

Uninstall XLMind Studio the same way as any Windows application, from **Settings › Apps › Installed apps**. Close Excel first.

---

Still stuck? [troubleshooting.md](troubleshooting.md) · [SUPPORT.md](../SUPPORT.md) · <support@xlmindstudio.com>
