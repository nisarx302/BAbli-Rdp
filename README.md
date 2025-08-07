---

<p align="center">
  <img src="https://i.postimg.cc/tT4NTGSL/7517439.webp" width="200"/>
</p>

<h1 align="center">
  🚀 <strong>TSun RDP Access using GitHub Actions & Ngrok</strong> 🖥️
</h1>

<p align="center">
  Easily set up free temporary Windows RDP access through GitHub Actions and Ngrok tunneling 💻
</p>

---

<h2 align="center">🌟 Unique Features</h2>

<p align="center">
  <img src="https://cdn-icons-png.flaticon.com/512/4415/4415631.png" width="50"/>
  <br/>
  <strong>💡 Zero Hosting Required</strong><br/>
  No need for any VPS or paid server to get started.
</p>

<p align="center">
  <img src="https://i.postimg.cc/RF2dbbdy/6815044.webp" width="50"/>
  <br/>
  <strong>⚙️ Easy Workflow Automation</strong><br/>
  Use GitHub Actions for a hands-off setup experience.
</p>

<p align="center">
  <img src="https://i.postimg.cc/HLgCJ1rr/9781674.webp" width="50"/>
  <br/>
  <strong>🔐 Secure Auth via GitHub Secrets</strong><br/>
  Your token stays safe and hidden from public eyes.
</p>

<p align="center">
  <img src="https://i.postimg.cc/nrQmwmpS/8306764.webp" width="50"/>
  <br/>
  <strong>⏱️ Instant RDP in Minutes</strong><br/>
  Workflow completes in under 3 minutes for fast access.
</p>

<p align="center">
  <img src="https://i.postimg.cc/x87wnwrT/7927010.webp" width="50"/>
  <br/>
  <strong>🌈 Aesthetic & Branded Setup</strong><br/>
  TSUN-branded UI, instructions, and terminal aesthetics.
</p>

# 🌐 Full Setup Guide

## 🌟 Prerequisites

Shuru karne se pehle, aapke paas ye cheezein honi chahiye:

* **GitHub Account**: Obviously! 🐙
* **Ngrok Account With CC Added**: Ngrok ki official website ([ngrok.com](https://ngrok.com)) par free account bana lein. Ye aapko ek authentication token provide karega.

---

## 🔑 Step 1: Get Your Ngrok Auth Token

1. Apne browser mein jayein: [Ngrok Dashboard](https://dashboard.ngrok.com/get-started/your-authtoken)
2. Login karein.
3. Apna **Auth Token** copy karein. Kuch is tarah ka hoga: `2k25_YOUR_TOKEN_HERE`

---

## 🔒 Step 2: Set `NGROK_AUTH_TOKEN` in GitHub Repository Secrets

1. Apne GitHub repository par jayein.
2. **Settings** > **Secrets and variables** > **Actions** pe click karein.
3. **New repository secret** pe click karein.
4. Name: `NGROK_AUTH_TOKEN`
5. Secret: Paste your Ngrok Auth Token
6. **Add secret** pe click karein.

---

## 💻 Step 3: Create GitHub Actions Workflow File

1. Apne repo mein `.github/workflows/rdp.yml` naam se file banayein.
2. Neeche diya gaya code paste karein:

```yaml
name: TSun RDP Access

on: [push, workflow_dispatch]

jobs:
  build:
    runs-on: windows-latest

    steps:
    - name: Download Ngrok 📅
      run: Invoke-WebRequest https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-windows-amd64.zip -OutFile ngrok.zip

    - name: Extract Ngrok 📦
      run: Expand-Archive ngrok.zip

    - name: Authenticate Ngrok 🔐
      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      env:
        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}

    - name: Enable Terminal Services (RDP) 🚀
      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0

    - name: Enable Remote Desktop Firewall Rule 🔥
      run: Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

    - name: Set RDP User Authentication 🛡️
      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1

    - name: Create RDP User Account 🧑‍💻
      run: |
        try {
            Get-LocalUser -Name "saeedxdie" | Out-Null
            Write-Host "User 'saeedxdie' already exists. Skipping creation."
        } catch {
            Write-Host "Creating new user 'saeedxdie'..."
            New-LocalUser -Name "saeedxdie" -Password (ConvertTo-SecureString "S@eedxdie1" -AsPlainText -Force) -AccountNeverExpires
            Add-LocalGroupMember -Group "Administrators" -Member "saeedxdie"
            Write-Host "User 'saeedxdie' created and added to Administrators group."
        }

    - name: Create Ngrok TCP Tunnel for RDP 🔗
      run: .\ngrok\ngrok.exe tcp 3389
```

3. File save karne ke liye "**Commit new file**" pe click karein.

---

## ▶️ Step 4: Run the Workflow & Connect to RDP

1. GitHub repo mein **Actions** tab pe jayein.
2. `TSun RDP Access` workflow pe click karein.
3. **Run workflow** button dabayein ya push kar ke workflow chalayen.
4. Workflow chalne ke baad `GoTo NGrok Select EndPoitn` step ke output mein tunnel URL aayega, jese:

```
tcp://0.tcp.ngrok.io:12345
```

5. Is URL ko copy karein.
6. Apne Windows PC pe Remote Desktop Connection app open karein.
7. **Computer** field mein `0.tcp.ngrok.io:12345` jesa address paste karein.
8. **Username**: `saeedxdie`, **Password**: `S@eedxdie1`

---

## ⚠️ Security Warning (Bohat Zaroori!)

> **Password ****************************`S@eedxdie1`**************************** hardcoded hai. Ye risky hai.**

### Recommendations:

* Password ko bhi **GitHub Secrets** mein store karein.
* RDP ke bajaye **SSH** ya kisi aur secure method ka istemal karein.
* Jab kaam ho jaye to **workflow disable** ya **delete** kar dein.

---

## 📌 Changelog

### Version: 2.0.5

* **Date**: 2025-08-07
* Fix YAML Errors And Now This Is Working 100%.
* Author: ༯𝙎ค૯𝙀𝘿✘🫀

### Version: 1.0.1

* **Date**: 2025-08-06
* YAML code block updated with language specifier for syntax highlighting.
* Author: ༯𝙎ค૯𝙀𝘿✘🫀

### Version: 1.0.0

* **Date**: 2025-08-06
* README ka initial version
* Detailed Ngrok, GitHub secrets, YAML, RDP connection guide
* Branding, emojis, and security notices added
* Author: ༯𝙎ค૯𝙀𝘿✘🫀

---
<p align="center">

 ## 🔗 My Portfolio & Socials

[![🌐 Linktree](https://img.shields.io/badge/Linktree-Portfolio-brightgreen?style=for-the-badge&logo=linktree)](https://linktr.ee/saeedxdie)
[![🤍 Gravatar](https://img.shields.io/badge/Gravatar-Profile-blueviolet?style=for-the-badge&logo=gravatar)](https://gravatar.com/cheerfuld27b01881a)
[![💻 GitHub](https://img.shields.io/badge/GitHub-Code-000?style=for-the-badge&logo=github)](https://github.com/SaeedX302)
[![📺 TikTok](https://img.shields.io/badge/TikTok-Fun-black?style=for-the-badge&logo=tiktok)](https://www.tiktok.com/@saeedxdie)
</p>
---

## ✨ Jaun Elia Touch

<i>Agar tumhe samajh na aaye meri khamoshi,<br>
To bekaar hai tumse koi guftagu karna.</i>

\~ <strong>Jaun Elia</strong> ❤️

<p align="center">
  <img src="https://i.postimg.cc/mkRzDYgh/image.png" width="150" style="border-radius: 10px"/>
</p>

---

<p align="center">
  Made with 🫀 by <strong>༯𝙎ค૯𝙀𝘿✘🫀</strong><br/>
  <small>Credits: °【〆༯𝙎ค૯𝙀𝘿】✘,【.ISHU.】</small>
</p>
