# Troubleshooting

Practical fixes for the situations people hit most often.

If none of this resolves your problem, contact <support@xlmindstudio.com> or use the [support form](https://xlmindstudio.com/en/support?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=troubleshooting_support) — and please read [SUPPORT.md](../SUPPORT.md) first so you know what not to include.

---

## Windows shows a SmartScreen notice when I run the installer

**What it means.** Windows SmartScreen does not judge a file by its digital signature alone. It also weighs *reputation*, which builds up as a given signed release is downloaded and installed over time. A recently published release can therefore still show a notice even though it is correctly signed by a verified publisher.

**What to do — verify, do not disable.**

1. Confirm you downloaded from **<https://xlmindstudio.com/en/download?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=troubleshooting_download>** and nowhere else.
2. Right-click the installer, open **Properties → Digital Signatures**, and confirm the publisher matches the one listed on the [security page](https://xlmindstudio.com/en/security).
3. Compare the SHA-256 checksum of your download against the value published for the current release. Full walkthrough: [security.md](security.md).
4. Only once both checks pass, continue with the installation.

**What not to do.** Do not turn off Windows security features or your antivirus in order to install software — any software. If the publisher or checksum does not match, the correct action is to stop and contact support, not to bypass the warning.

---

## My antivirus quarantined XLMind Studio

**Do not switch your antivirus protection off.**

Send the following to <support@xlmindstudio.com> so the detection can be investigated and, where appropriate, reported to the security vendor:

- The security product name and version
- The exact detection name or code it displayed
- Your XLMind Studio version and Windows version

---

## The XLMind Data / XLMind Plus tabs are not in Excel

Work through these in order.

**1. Is your Excel 64-bit?**

XLMind Studio supports 64-bit desktop Excel on Windows only. Check **File → Account → About Excel**; the version line ends in 32-bit or 64-bit. A 32-bit Office installation cannot load the add-in.

**2. Are you using desktop Excel?**

Excel for the web, Excel for macOS and mobile Excel apps are not supported.

**3. Was Excel open during installation?**

Close Excel completely, then reopen it. If the tabs still do not appear, reinstall with Excel closed.

**4. Has Excel disabled the add-in?**

Excel disables add-ins after a crash or a slow load. Go to **File → Options → Add-ins**, set **Manage** to **COM Add-ins** and select **Go**, then make sure XLMind Studio is ticked. Also check **Manage → Disabled Items → Go** and re-enable XLMind Studio if it is listed there.

**5. Is a company policy blocking it?**

On managed devices, add-in loading can be restricted by policy. Your IT team may need to approve the installation.

If none of that works, send support your Windows version, your Excel version and architecture, and your XLMind Studio version.

---

## Installation needs administrator permission

Administrator permission may be required depending on your Windows settings and company policies. On a corporate device, ask your IT team to approve the installation. For deployment across many devices, choose **Corporate Quote** on the [support page](https://xlmindstudio.com/en/support?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=troubleshooting_corporate).

---

## My license code will not activate

- Check the code against the email you received — license codes are delivered **only by email**, to the address used for the purchase, and are never shown on the payment confirmation page.
- If the email has not arrived, look in your spam and junk folders first, then contact support with your order number (never post it publicly).
- Activation needs internet access. If you are behind a corporate proxy or firewall, that may be what is blocking it.
- A Single License allows one active device at a time; the 5-license plan allows up to five. If the device limit is full, activating on a new machine automatically deactivates the oldest active device.

---

## A tool did something I did not expect

- Use **Undo Last** in the **Quick** group on the **XLMind Data** ribbon to reverse the most recent XLMind Studio operation.
- Try the tool again on a small copy of the data to see exactly what it changes.
- As a habit, save before running a tool that rewrites a large range.

If the behaviour looks like a genuine bug, open a bug report in this repository with your XLMind Studio, Windows and Excel versions, the tool name, what you expected, what happened, and steps to reproduce — using sample data, never a confidential workbook.

---

## Excel is slow after installing

- Very large ranges take longer; try selecting only the range you need rather than whole columns.
- Other add-ins running at the same time can compound the effect. Temporarily disabling other COM add-ins helps identify the cause.
- If a specific tool is consistently slow on a specific shape of data, that is worth reporting — include the row and column counts.

---

## Updating

Use the update check inside the product, or the update link provided to you. Back up important workbooks before installing an update. Update eligibility is described in the [Update & Support Policy](https://xlmindstudio.com/en/updates-and-support).
