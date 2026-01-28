---

# Linux Server Troubleshooting & System Administration Journal

This repository documents my **hands-on Linux server learning journey**, focused on **real troubleshooting, system recovery, and practical administration tasks**.

The goal of this repo is simple:
👉 show *how I think, diagnose problems, and fix systems* — not just what commands I know.

This is written as both:

* a **learning journal**
* a **portfolio-style proof of skills** for jobs and internships

---

## 🧠 Why this repository exists

Instead of just reinstalling Linux whenever something broke, I chose to:

* investigate the issue
* isolate the problem
* recover the system safely
* document the entire process

This mirrors how **real Linux servers** are managed in production environments.

---

## 🖥️ Environment

* Host OS: Windows
* Virtualization: VirtualBox
* Guest OS: Ubuntu Desktop (used like a server)
* Access methods:

  * GUI (GNOME)
  * TTY (Ctrl + Alt + Fx)
  * Terminal emulators (gnome-terminal, xterm)

---

## 🚨 Major Issue Encountered

### Problem: GNOME Terminal not opening

After installing Ubuntu:

* GUI login worked normally
* Desktop loaded successfully
* **GNOME Terminal failed to open**

  * Click → loading icon → disappears
  * `Ctrl + Alt + T` did nothing
  * `Alt + F2 → gnome-terminal` failed

This happened **even after multiple restarts**.

---

## 🔍 Initial Observations

* System was not frozen
* GUI was responsive
* Keyboard shortcuts worked
* This suggested the issue was **application-level**, not OS-level

---

## 🧯 Recovery & Troubleshooting Steps

### 1️⃣ Switched to TTY (Out-of-band access)

```bash
Ctrl + Alt + F3
```

Logged in successfully.

This confirmed:

* Kernel is running
* User accounts are functional
* System is accessible without GUI

> This is similar to how cloud servers are accessed when GUI tools fail.

---

### 2️⃣ Identified a permission issue (sudo failure)

When running:

```bash
sudo apt update
```

Error:

```
sysadmin is not in the sudoers file
```

This indicated:

* The user lacked administrative privileges
* Package management was blocked for that user

This is a **critical real-world issue**, especially on servers.

---

### 3️⃣ Gained root access for system repair

```bash
su -
```

Root access worked, confirming:

* System permissions were intact
* The issue was user / configuration related

---

### 4️⃣ Tested GNOME Terminal directly

```bash
gnome-terminal
```

Result:

* Terminal exited immediately
* No visible error output

This confirmed:

* GNOME Terminal binary or dependencies were broken

---

### 5️⃣ Reinstalled GNOME Terminal stack

```bash
apt install --reinstall gnome-terminal gnome-terminal-data libvte-2.91-0 -y
```

Purpose:

* Restore terminal binary
* Restore VTE backend (common failure point)

Result:

* Packages reinstalled successfully
* Issue **still persisted**

Documenting failed attempts is intentional — this shows real troubleshooting.

---

### 6️⃣ Reset GNOME user configuration

```bash
dconf reset -f /org/gnome/terminal/
dconf reset -f /org/gnome/
```

Purpose:

* Clear corrupted user preferences
* Remove broken terminal profiles

Result:

* GNOME Terminal still failed

---

### 7️⃣ Installed alternative terminal to validate system health

```bash
apt install xterm -y
```

Launched via:

```bash
Alt + F2
xterm
```

✅ **xterm opened successfully**

This confirmed:

* Shell is working
* Networking is working
* Package manager works
* User environment is usable

The issue was **isolated strictly to GNOME Terminal**.

---

## ✅ Final Diagnosis

* Ubuntu OS: Healthy
* Kernel: Healthy
* Networking: Working
* Users & permissions: Working
* GUI session: Working
* ❌ GNOME Terminal: Broken

This is a **non-critical, recoverable issue**, not a system failure.

---

## 💡 Key Lessons Learned

* Always isolate the problem before reinstalling
* TTY access is essential for server recovery
* A broken GUI tool does **not** mean a broken system
* Alternative tools (like xterm) are valuable diagnostics
* Permission misconfiguration can block critical workflows

---

## 🧑‍💻 Why this matters for jobs

This repo demonstrates:

* Linux troubleshooting mindset
* Calm, step-by-step diagnosis
* Understanding of permissions and privilege escalation
* Server-style thinking (CLI-first, GUI optional)
* Clear technical documentation

These skills apply directly to:

* Linux system administration
* Cloud support roles
* DevOps / SRE internships
* Technical support engineering

---

## 📌 Notes on Screenshots

Some troubleshooting steps were performed without screenshots due to clipboard reset during reinstall.
This repository prioritizes **accuracy, reasoning, and reproducibility** over visuals.

Where available, installation screenshots will be added later.

---

## 🚀 Next Steps (Planned)

* Rebuild system with clean sudo configuration
* Convert setup into SSH-only “server mode”
* Practice firewall and networking rules
* Repeat same workflow on a cloud VM
* Add automation scripts (Bash)

---

## 📚 Disclaimer

This repository reflects **real learning in progress**.
Mistakes, failed attempts, and resets are intentionally documented — because that’s how real systems are learned and managed.

---

just tell me what you want next 👌
