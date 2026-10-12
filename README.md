# Windows 11 IoT Enterprise LTSC 2024: Convert Evaluation → Full License

> **Updated 2026 edition.** Turn your Windows 11 **IoT Enterprise LTSC 2024 Evaluation**
> into a fully licensed copy — **no reinstall, no data loss**, ~5 minutes.

---

## What you need

- Windows 11 **IoT Enterprise LTSC 2024** (evaluation) already installed
- An **Administrator** account
- A **genuine** IoT Enterprise LTSC 2024 product key
- Stable internet connection

---

## Step 1 — Check your edition

Open **Command Prompt as Administrator** and run:

```cmd
winver
slmgr /dlv
```

Look at the line that says **Operating System / Edition**:

- ✅ Shows `Windows(R), IoTEnterpriseS` → **the SKU is already installed, skip Step 2**.
- ❌ Shows `Core` / `Professional` / `Evaluation` → do **Step 2**.

> Since late 2024, the official Microsoft **evaluation ISO already includes the
> IoT Enterprise LTSC 2024 edition**, so most people skip Step 2 entirely.

---

## Step 2 — Install the IoT Enterprise SKU (only if Step 1 fails)

1. Grab the included [`assets/IoTEnterpriseS.zip`](assets/IoTEnterpriseS.zip) from this repo.
2. Extract it and copy the **`IoTEnterpriseS`** folder into:

   ```
   C:\Windows\System32\spp\tokens\skus
   ```

3. Reinstall the licenses:

   ```cmd
   cscript /nologo %windir%\system32\slmgr.vbs /rilc
   ```

4. Go back to **Step 1** and confirm it now shows `IoTEnterpriseS`.

---

## Step 3 — Clear the evaluation license

Run these **in order** (Admin prompt, copy-paste one by one):

```cmd
cscript /nologo %windir%\system32\slmgr.vbs /upk
cscript /nologo %windir%\system32\slmgr.vbs /ckms
cscript /nologo %windir%\system32\slmgr.vbs /cpky
sc config LicenseManager start= auto
net start LicenseManager
sc config wuauserv start= auto
net start wuauserv
```

| Command | What it does |
|---|---|
| `/upk` | Removes the evaluation key |
| `/ckms` | Clears KMS server pointers |
| `/cpky` | Deletes the cached key from the registry |
| `sc` / `net` | Makes sure the licensing and Windows Update services are running |

---

## Step 4 — Install your genuine key

**Easiest way:** `Settings → System → Activation → Change product key`,
enter your genuine **IoT Enterprise LTSC 2024** key and follow the prompts.

**Or from the command line:**

```cmd
cscript /nologo %windir%\system32\slmgr.vbs /ipk <YOUR-KEY>
cscript /nologo %windir%\system32\slmgr.vbs /ato
```

---

## Step 5 — Verify

```cmd
slmgr /dlv
slmgr /xpr
```

You should see:

- 🟢 **License Status: Licensed**
- 🟢 **Windows is permanently activated** (no expiry date)

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `The generic key is not used for this edition` | The GVLK only works with the `IoTEnterpriseS` SKU → complete **Step 2** first. |
| `slmgr /xpr` still shows an expiry | The eval key was not fully removed → repeat **Step 3** in order, then **Step 4**. |
| Activation error `0xC004F074` | KMS couldn't be reached — wait or activate with a **retail / MAK** key via `Settings`. |
| Edition still shows "Evaluation" after Step 2 | Double-check the folder is `C:\Windows\System32\spp\tokens\skus\IoTEnterpriseS` and rerun `/rilc`. |

---

## FAQ

**Do I need to reinstall Windows?**
No. Everything — apps, files, settings — stays untouched.

**What if I do nothing?**
The evaluation keeps nagging you to activate and **expires (~90 days, rearmable)**,
with limited enterprise features.

**Is the generic GVLK key enough?**
It stops the evaluation but **does not activate** the system. You still need a
**genuine key** to activate.

**Can I switch back to a standard edition later?**
No — IoT Enterprise LTSC is a different product family. You'd need a clean install.

---

## Source

Updated from the original [ProSoftKeys tutorial](https://prosoftkeys.com/convert-windows-11-iot-enterprise-ltsc-evaluation-to-version/).

---

## `$ ls ~/contacto`

[![X](https://img.shields.io/badge/X-@madmasx-000000?style=for-the-badge&logo=x&logoColor=E63946&labelColor=0d1117&color=E63946)](https://x.com/madmasx)
[![GitHub](https://img.shields.io/badge/GitHub-madmasx-000000?style=for-the-badge&logo=github&logoColor=E63946&labelColor=0d1117&color=E63946)](https://github.com/madmasx)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy_Me_a_Coffee-apóyame-000000?style=for-the-badge&logo=buymeacoffee&logoColor=E63946&labelColor=0d1117&color=E63946)](https://buymeacoffee.com/madmasx)