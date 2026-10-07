# NixGuard Desktop for Windows — installer build of the NixGuard AI SOC desktop client (v0.0.1)

Windows installer build of the NixGuard desktop client: sign in to NixGuard and install or remove the Wazuh endpoint agent on your PC.

> **Early build.** This repository holds a single packaged build (v0.0.1, built June 2024). It contains no source code, no release notes and no automatic updates. See [What we could not verify](#what-we-could-not-verify) before installing.

## What is it

[NixGuard](https://nixguard.com) is an AI-native active security operations center (AI SOC) and continuous compliance platform built on Wazuh open-source SIEM/XDR telemetry. This repository distributes a prebuilt Windows desktop client for NixGuard, packaged with Electron and electron-builder.

The repository contains one file plus Git LFS configuration:

| File | What it is |
|---|---|
| `nixguard-windows-desktop-app-v0.0.1.zip` | Build output, stored with [Git LFS](https://git-lfs.com). About 139 MiB. |
| `.gitattributes` | Tells Git to store `*.zip` files in Git LFS. |

Inside the zip, in the folder `nixguard-windows-desktop-app-v0.0.1/`:

| Path | What it is |
|---|---|
| `NixGuard by NEX Labs Setup 1.0.0.exe` | NSIS installer (app version 1.0.0). |
| `win-unpacked/NixGuard by NEX Labs.exe` | The same app, unpacked, runnable without installing. |
| `builder-effective-config.yaml`, `builder-debug.yml`, `*.blockmap` | electron-builder build metadata. Not needed to run the app. |

## Features

What the packaged app does, based on inspecting the files bundled in the build:

- **NixGuard sign-in.** Log in with your NixGuard account; the session is kept in the Windows Credential Manager until the token expires.
- **Wazuh agent install.** Enter an agent name (no spaces) and the app downloads the official Wazuh agent 4.7.4 MSI from `packages.wazuh.com` and installs it silently, enrolled with your NixGuard environment, so the PC sends endpoint security telemetry for threat detection.
- **Wazuh agent removal.** Stops the Wazuh service and uninstalls the agent.
- **Agent status.** Detects whether the Wazuh agent is installed and shows **Install Security Agent** or **Delete Security Agent** accordingly.
- **Open the web dashboard.** The **NixGuard Web Dashboard** button opens the NixGuard web app in your default browser.

## Requirements

- Windows, 64-bit (x64). The build targets x64 only.
- Administrator rights. The app requests elevation every time it starts (`requireAdministrator`), because it installs and removes a Windows service.
- A NixGuard account. See [nixguard.com](https://nixguard.com).
- Internet access to NixGuard and to `packages.wazuh.com`.
- [Git LFS](https://git-lfs.com) if you download by cloning.

## Installation

### 1. Download

Either click `nixguard-windows-desktop-app-v0.0.1.zip` in the file list on GitHub and choose **Download raw file**, or clone with Git LFS:

```bash
git lfs install
git clone https://github.com/thenexlabs/nixguard-desktop-windows-build.git
```

Without Git LFS, a clone contains only a small text pointer file instead of the zip.

### 2. Verify the download

The SHA-256 of the zip (from its Git LFS pointer) is:

```
4d393d66ec5b8c0fb736bd185a9ad95e24dc9de7dea2ccccbdb4ffb1ca5a4713
```

In PowerShell:

```powershell
Get-FileHash .\nixguard-windows-desktop-app-v0.0.1.zip -Algorithm SHA256
```

### 3. Install

1. Extract the zip.
2. Run `NixGuard by NEX Labs Setup 1.0.0.exe` from the extracted `nixguard-windows-desktop-app-v0.0.1` folder and follow the installer.
3. The installer and the app are **not code-signed**, so Windows SmartScreen may show a warning. Only continue if the SHA-256 above matches.

To run without installing, start `win-unpacked\NixGuard by NEX Labs.exe` instead.

## Usage

1. Start **NixGuard by NEX Labs** and accept the administrator prompt.
2. Sign in with your NixGuard account.
3. To protect this PC, click **Install Security Agent**, enter a name for the agent (for example `MSI-Laptop`, no spaces), and click **Install + Set Up**. Installation runs in the background; the status updates after a few seconds.
4. **Delete Security Agent** uninstalls the Wazuh agent. **NixGuard Web Dashboard** opens NixGuard in your browser. **Log Out** ends the session.

To uninstall the desktop app itself, use **Settings → Apps** in Windows.

## What we could not verify

- The app was not run or installed while writing this README; the features above come from reading the bundled files, not from testing.
- Whether this build still works with the current NixGuard service.
- macOS and Linux: the build targets Windows only, and the bundled agent installer is implemented for Windows only.
- The source code of the desktop client is not in this repository.

## Security

To report a vulnerability, email the security contact listed on [nixguard.com](https://nixguard.com). Please don't open a public issue.

## About NEX Level Labs

NEX Level Labs Inc. is a deeptech cybersecurity company. It builds [NixGuard](https://nixguard.com), an AI-native active SOC and continuous compliance platform. Learn more at [thenex.world](https://thenex.world).
