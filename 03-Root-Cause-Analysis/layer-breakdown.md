# Layer Breakdown (Why This Failed)

This incident looked like a “broken Linux distro,” but it was not.

## What was actually failing
The failure was triggered by the **WSL Microsoft Store / AppX package layer** attempting a repair (MSI self-heal) and failing, producing:
- “The feature you are trying to use is on a network resource that is unavailable”
- “Fatal error during installation of Windows Subsystem for Linux”

## What was NOT failing
Evidence showed the core system layers were intact:
- **Kali distro** still registered (`wsl -l -v` showed `kali-linux`)
- **WSL2** still in use (Version 2)
- Windows servicing layer was not the suspected root cause for this specific symptom pattern

## Architecture Layers (mental model)

Windows 11 Canary (Build 26200.7840)
│
├── Windows Servicing / Component Store (DISM)
├── Virtualization Layer (VirtualMachinePlatform / Hyper-V plumbing)
├── WSL Kernel Layer
├── Microsoft Store / AppX Packaging Layer  ← Failure occurred here
└── Linux Distro Filesystem (kali-linux)   ← Intact

## Why this matters
Correctly identifying the failing layer prevents destructive actions like:
- unregistering the distro
- reinstalling Windows
- deleting filesystems

Instead, remediation can target the packaging layer while preserving the distro.
