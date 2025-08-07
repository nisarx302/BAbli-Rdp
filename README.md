# TSun RDP Access using GitHub Actions & Ngrok 🖥️

Ye repository aapko guide karega ke kaise aap ek temporary Remote Desktop Protocol (RDP) 
connection set up kar sakte hain GitHub Actions ke through, 
Ngrok service ka use karte hue. 
Is se aapko GitHub ke Windows runners par remote access mil jayega. 🌐

---

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
      # Pehle user banayenge agar woh exist nahi karta
      run: |
        try {
            Get-LocalUser -Name "saeedxdie" | Out-Null
            Write-Host "User 'saeedxdie' already exists. Skipping creation."
        } catch {
            Write-Host "Creating new user 'saeedxdie'..."
            # 'UserMayChangePassword' parameter ko remove kar diya gaya hai kyunki yeh is PowerShell version mein support nahi hota.
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
4. Workflow chalne ke baad 
`GoTo NGrok Select EndPoitn ` step ke output mein tunnel URL aayega, jese:

   ```
   tcp://0.tcp.ngrok.io:12345
   ```
5. Is URL ko copy karein.
6. Apne Windows PC pe Remote Desktop Connection app open karein.
7. **Computer** field mein `0.tcp.ngrok.io:12345` jesa address paste karein.
8. **Username**: `saeedxdie`, **Password**: `S@eedxdie1`

---

## ⚠️ Security Warning (Bohat Zaroori!)

> **Password `S@eedxdie1` hardcoded hai. Ye risky hai.**

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

## 🔗 My Portfolio & Socials

* 🌐 [Linktree Portfolio](https://linktr.ee/saeedxdie)
* 🤍 [Gravatar Profile](https://gravatar.com/cheerfuld27b01881a)
* 💻 [GitHub](https://github.com/SaeedX302)
* 📺 [TikTok](https://www.tiktok.com/@saeedxdie)

---

*Agar tumhe samajh na aaye meri khamoshi,
To bekaar hai tumse koi guftagu karna.*
\~ **Jaun Elia**
