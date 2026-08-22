# Security and installer verification

How XLMind Studio is signed and distributed, and how to verify a download before you install it.

To report a suspected vulnerability, see [SECURITY.md](../SECURITY.md) — please do not open a public issue.

## The short version

- XLMind Studio installers are **digitally signed** with a code-signing certificate.
- Each release is published with a **SHA-256 checksum**.
- The **official website is the only canonical download source**. Installers are not published as GitHub Releases.
- Your Excel data stays on your computer for XLMind Studio’s local processing.

Publisher details, certificate authority, current version and current SHA-256 value are maintained on the official security page:

- **English:** <https://xlmindstudio.com/en/security>
- **Türkçe:** <https://xlmindstudio.com/tr/guvenlik>

They are deliberately not copied into this repository, because a stale checksum is worse than no checksum.

## Verify a download in three steps

### 1. Check where it came from

Download only from **<https://xlmindstudio.com/en/download?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=security_doc_download>**.

If you received an XLMind Studio installer as an email attachment, from a colleague, from a download portal or from a file-sharing site, do not run it. Delete it and download a fresh copy from the official site.

### 2. Check the digital signature

1. Right-click the downloaded installer and choose **Properties**.
2. Open the **Digital Signatures** tab.
3. Select the signature in the list and choose **Details**.
4. Confirm that the publisher name matches the publisher listed on the [security page](https://xlmindstudio.com/en/security), and that Windows reports the signature as OK.

If the **Digital Signatures** tab is missing, or the publisher does not match, stop. The file is not a genuine XLMind Studio installer, or it was modified after it was published.

### 3. Compare the SHA-256 checksum

Open **Windows PowerShell**, and run the following against the file you downloaded:

```powershell
Get-FileHash -Algorithm SHA256 "$HOME\Downloads\<installer-file-name>.exe"
```

Compare the value it prints with the SHA-256 value published for the current release on the [security page](https://xlmindstudio.com/en/security). The comparison is not case-sensitive, but every character must match.

If the two values differ, do not install the file. Delete it, download again from the official site, and if the mismatch persists contact <support@xlmindstudio.com>.

## Why Windows may still show a warning

A valid signature and a SmartScreen notice are not contradictory. Windows SmartScreen does not look at the digital signature alone — it also weighs **reputation**, which accumulates as a given signed release is downloaded and installed over time. A recently published release can therefore still trigger a notice while it builds that reputation.

The right response is to verify rather than to disable protection:

- Confirm the publisher in **Properties → Digital Signatures**.
- Confirm the SHA-256 checksum against the security page.
- Never turn off your antivirus or Windows security features to install software.

More detail, and what the notice looks like: [troubleshooting.md](troubleshooting.md).

## If a security product quarantines XLMind Studio

**Do not switch your antivirus protection off.** Instead, send the following to <support@xlmindstudio.com> so the detection can be investigated and reported to the vendor:

- The security product name and version
- The exact detection name or code it displayed
- Your XLMind Studio version
- Your Windows version

## How your data is handled

- Your Excel data stays on your computer for XLMind Studio’s local processing.
- Workbook files, cell contents and worksheet names are not sent to XLMind Studio servers during normal product use.
- Excel content is not sent to or shared with artificial intelligence services.
- Internet access may be required for licensing and activation, payment, download, update checks, in-product information messages and support.
- The limited data needed for licensing, sales and support is processed under the published [Privacy Policy](https://xlmindstudio.com/en/privacy-policy) and [Personal Data Protection Notice](https://xlmindstudio.com/en/personal-data-protection).

## Corporate deployment

On a managed device, installation may require IT approval. IT teams evaluating XLMind Studio can use the [security page](https://xlmindstudio.com/en/security?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=security_doc_security) for publisher and certificate details, and the [system requirements page](https://xlmindstudio.com/en/system-requirements?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=security_doc_sysreq) for the verified compatibility matrix. For licensing across a larger number of devices, choose **Corporate Quote** on the [support page](https://xlmindstudio.com/en/support?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=security_doc_support).
