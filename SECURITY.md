# Security Policy

XLMind Studio is a Windows desktop add-in for Microsoft Excel, developed by BYMMB Bilişim Akademi ve Ticaret Limited Şirketi. This document explains how XLMind Studio is distributed and verified, and how to report a security concern responsibly.

This repository contains public documentation only. It does not contain XLMind Studio source code.

## Reporting a security issue

**Please do not open a public GitHub issue for a security vulnerability.** A public issue exposes the problem before a fix is available.

Instead, email **<support@xlmindstudio.com>** with `Security` in the subject line, or use the contact form on the [support page](https://xlmindstudio.com/en/support?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=security_policy_support).

Please include, as far as you can:

- A description of the issue and why you believe it is a security problem
- The XLMind Studio version affected (shown in the product)
- Your Windows and Microsoft Excel version and architecture
- Reproduction steps, and a proof of concept if you have one
- Any log output or error text

Please **do not** include license keys, order numbers, payment details, personal data or confidential workbooks in your report. If a sample file is genuinely required, say so and it will be arranged through support.

Reports are answered from the same support channel as other requests; replies are usually quick on business days. Please give a reasonable period for investigation and remediation before disclosing an issue publicly.

## Scope

**In scope**

- The XLMind Studio add-in for Microsoft Excel
- The official XLMind Studio installer distributed from <https://xlmindstudio.com>
- The XLMind Studio website, licensing and activation flow

**Out of scope**

- Microsoft Excel itself, Windows, and any third-party software
- Builds, repackaged installers or "cracked" versions obtained from anywhere other than the official website
- Issues that require an already fully compromised machine
- Reports generated purely by automated scanners with no demonstrated impact

## Supported versions

Security fixes are delivered in the current XLMind Studio release. If you are reporting an issue, please confirm it against the latest version available from the [official download page](https://xlmindstudio.com/en/download?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=security_policy_download).

## How XLMind Studio is distributed

The official website is the **only** canonical download source for XLMind Studio installers:

- <https://xlmindstudio.com/en/download>

Installers are **not** published as GitHub Releases and are not distributed through third-party download portals, mirrors or file-sharing sites. A copy of XLMind Studio obtained from anywhere else cannot be verified and should not be trusted.

## Installer integrity

Every XLMind Studio release is:

- **Digitally signed** with a code-signing certificate, so Windows can show you the publisher before you install;
- **Published with a SHA-256 checksum**, so you can confirm the exact bytes you downloaded.

The publisher name, certificate authority, current version and current SHA-256 value are maintained on the official security page rather than duplicated here, so that they never fall out of date:

- English: <https://xlmindstudio.com/en/security>
- Türkçe: <https://xlmindstudio.com/tr/guvenlik>

A step-by-step verification walkthrough is in [docs/security.md](docs/security.md).

## Data handling

XLMind Studio processes workbook data locally:

- Your Excel data stays on your computer for XLMind Studio's local processing.
- Workbook files, cell contents and worksheet names are not sent to XLMind Studio servers during normal product use.
- Excel content is not sent to or shared with artificial intelligence services.
- Internet access may be required for licensing and activation, payment, download, update checks, in-product information messages and support.

The limited data needed for licensing, sales and support is processed under the published [Privacy Policy](https://xlmindstudio.com/en/privacy-policy) and [Personal Data Protection Notice](https://xlmindstudio.com/en/personal-data-protection).

## Antivirus detections

If a security product quarantines XLMind Studio, **do not turn your antivirus protection off.** Send the security product name, the exact detection name or code it displayed, and your XLMind Studio version to <support@xlmindstudio.com> so the detection can be investigated and reported to the vendor.

---

Copyright © BYMMB Bilişim Akademi ve Ticaret Limited Şirketi. All rights reserved. XLMind Studio is proprietary commercial software.
