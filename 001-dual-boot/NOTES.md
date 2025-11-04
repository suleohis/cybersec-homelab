# Lab 001 – Dual-Boot Baseline (Ubuntu 24.04 + Windows 11)

**Date**: 2025-11-04  
**Hardware**: [Your Laptop Model]  
**Status**: ✅ **COMPLETE – Bootable + Baselines Captured**  
**Time to Resolve**: 42 min (including GRUB EFI recovery)

---

## Objective
Stand up a **bare-metal hybrid lab** and capture **forensic-grade baselines** for both OSes.

---

## Partition Layout (Verified via `lsblk -f`)
| Partition | Size | FS   | Role |
|----------|------|------|------|
| `/dev/sda1` | 100 MB | vfat | EFI System Partition (ESP) |
| `/dev/sda7` | 210 GB | ext4 | **Ubuntu root** (active) |
| *Others* | — | — | Windows + recovery |

---

## GRUB Recovery Summary
- **Initial Issue**: `cannot find EFI directory`  
- **Root Cause**: ESP not mounted in `chroot`  
- **Fix**:
  ```bash
  sudo mount /dev/sda7 /mnt
  sudo mount /dev/sda1 /mnt/boot/efi
  sudo mount -t devpts devpts /mnt/dev/pts
  grub-install --target=x86_64-efi --efi-directory=/boot/efi