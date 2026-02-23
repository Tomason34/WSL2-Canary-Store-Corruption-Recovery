# Controlled Recovery Procedure

## Objective
Repair the corrupted WSL Microsoft Store package layer without deleting the Kali Linux distribution.

---

## Step 1 — Confirm Distro Still Exists

```powershell
wsl -l -v

Observed:
NAME          STATE     VERSION
kali-linux    Stopped   2

Interpretation:
The Linux filesystem remained intact.
The failure was not a destructive unregistration event.

Step 2 — Remove Corrupted Store Package
Get-AppxPackage *WindowsSubsystemForLinux* | Remove-AppxPackage
Purpose:
Remove the broken Microsoft Store AppX registration layer.

This does NOT delete Linux distributions.

Step 3 — Reinstall WSL Platform
wsl --install
Behavior:
Reinstalled WSL platform
Installed default Ubuntu distro
Existing Kali distribution remained registered

Step 4 — Restore Kali as Default
wsl --shutdown
wsl -s kali-linux
Purpose:
Reset default distribution back to Kali.

Step 5 — Validate Toolchain Integrity
Inside Kali:
nmap --version
which nmap
ls ~
Result:
Pentesting tools intact
SSH keys preserved
Wordlists preserved
Lab artifacts preserved
No data loss occurred.
Commit message:
fix: document controlled WSL recovery procedure


---

After you commit that, your repo will now show:

- Incident validation
- Layer isolation
- Controlled repair
- Toolchain verification



