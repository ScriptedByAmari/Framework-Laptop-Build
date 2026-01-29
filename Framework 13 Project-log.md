Framework Laptop 13 DIY Build & Dual-Boot Project

📌 Project Overview

This project documents my hands-on assembly and configuration of a Framework Laptop 13 (Intel Core Ultra Series 1).

Goals:
- Assemble modular laptop hardware from components
- Set up a dual-boot system (Windows 11 + Ubuntu)
- Troubleshoot hardware/software compatibility issues
- Demonstrate job-relevant IT skills (hardware, OS config, BIOS, dual-booting, system recovery)

Reasoning for Dual-Boot Setup:
- Career Relevance: Many IT support, infrastructure, and sysadmin environments involve working across both Windows and Linux. This setup lets me practice administration, troubleshooting, and configuration in both platforms.
- Full Hardware Access: Running Linux on bare metal provides full performance and access to hardware-level features, unlike in VMs or WSL where virtualisation overhead and abstraction can limit certain operations.
- Real-World Tasks: Enables hands-on learning of partition management, bootloader configuration, BIOS/UEFI interactions, and OS recovery processes — all of which are relevant for enterprise support work.
- Skill Demonstration: Shows the ability to plan, implement, and troubleshoot a multi-OS environment, preparing me for scenarios where cross-platform support is required.

This serves as both a personal learning project and a technical portfolio piece relevant to roles in IT support, infrastructure, or system administration.

🖥️ Parts List / Specifications

Component | Details
--- | ---
Laptop Model | Framework Laptop 13 DIY Edition (Intel Core Ultra Series 1)
CPU | Intel Core Ultra 7 155H
RAM | 32 GB DDR5-5600 (2×16 GB, dual-channel)
Storage | 2 TB WD_BLACK SN850X NVMe SSD (PCIe Gen 4×4, high-performance, dual-boot optimised)
Expansion Cards | 2× USB-C, 1× USB-A, 1× DisplayPort (Ethernet TBD)
OS Setup | Dual-Boot: Windows 11 Home + Ubuntu 24.04 LTS

🔧 Build Log

(Based on the official Framework Quick Start Guide: https://guides.frame.work/Guide/Framework+Laptop+13+(Intel+Core+Ultra+Series+1)+DIY+Edition+Quick+Start+Guide/332?lang=en)

Step-by-Step Assembly:
1. Installed RAM modules in both slots for dual-channel performance
   Tip: Dual-channel provides better memory bandwidth now, making the system more responsive for multitasking and virtual machines.
   Note: On my unit, Channel 0 was the left-hand side slot and had the reversed notch. This meant the chips needed to face down for that slot — the opposite of what some guides may imply.
   It’s essential to visually confirm slot orientation before inserting DDR5 RAM to avoid forcing or misalignment.
   When upgrading in future, I can resell these sticks to offset the cost.
2. Installed 2 TB WD_Black SN850X NVMe SSD
   Tip: Chosen over the 1 TB SN770 for both performance (faster sequential and random speeds) and capacity (~150% more usable space post-OS/tool install). The larger drive allows more headroom for OS partitions, VM images, and project storage, reducing upgrade pressure.
4. Attached input cover and connected keyboard ribbon cables
5. Inserted expansion cards (USB-C, USB-A, DisplayPort)
6. Screwed in the bezel and secured chassis
7. Powered on and confirmed POST

 Photo placeholders (ready to drag/drop later):
- Unboxing
- Installing RAM
- SSD installation
- Expansion cards in place
- First POST / BIOS screen

## 🔐 Pre-Install Backup Strategy

To protect my Windows setup before attempting the Ubuntu dual-boot installation, I created a full system image backup using **AOMEI Backupper Standard** (free edition):

- ✅ **System Image**: Backed up the full Windows 11 OS and all EFI/system partitions to an external drive with sufficient space
- ✅ **Recovery Media**: Created a bootable USB recovery drive via AOMEI in case the OS became unbootable
- 🧠 **File Safety**: My external drive already had files on it; I created a separate backup folder (`FrameworkBackup_2025-09`) to avoid overwriting anything
- 🧪 **Tested Restore Plan**: Verified that I could boot into the recovery USB and access the image if a restore was needed

This gave me the confidence to proceed with dual-boot partitioning, knowing I could fully revert if needed.

## 📷 Photos Gallery

This section contains visual documentation of the build process. Images are organised by step for clarity and ease of reference.

### Step 1 – Unboxing

![Photo showing unopened parts and packaging](photos/Unboxing.jpg)

### Step 2 – Installing RAM

![Photo of memory sticks being inserted, showing left-side reversed notch](photos/RAM%20Installation.jpg)

### Step 3 – SSD Installation

![Photo of WD_BLACK SN850X being seated into the M.2 slot](photos/SSD%20Installation.jpg)

### Step 4 – Expansion Cards Inserted

![Photo of USB-C, USB-A, and DisplayPort cards installed in chassis](photos/Expansion%20Card%20Installation.jpg)

### Step 5 – First POST / BIOS Screen

![Photo of successful power-on with BIOS setup visible](photos/BIOS%20Screen.jpg)


💾 Operating System Setup

Partitioning & Prep
- Initial plan was to use Ventoy on a 32 GB USB stick to load both Ubuntu 24.04 and Windows 11 ISOs.
  - Multiple installation attempts failed on both old and brand-new 32 GB USB sticks, even after running as administrator, using DiskPart to wipe the drive, and disabling antivirus protection. 
  - Each attempt resulted in mid-process errors, leaving the USB unreadable and unformatable via standard Windows tools.
  - Decided to abandon Ventoy due to repeated reliability issues in favour of a more predictable, tool-specific method.
- Created two separate dedicated bootable USB sticks using Rufus:
  1. **Windows 11 USB** – Prepared via Rufus using GPT/UEFI partition scheme, NTFS file system, and with BitLocker automatic device encryption disabled for dual-boot compatibility. Also configured to bypass TPM/online account requirements for flexibility.
  2. **Ubuntu 24.04 USB** – Prepared via Rufus using GPT/UEFI partition scheme and FAT32 file system for maximum compatibility.
- This dual-stick approach ensures each OS has its own dedicated bootloader, reducing the chance of conflicts and simplifying the installation process.
- Planned installation order: Install Windows 11 first, then Ubuntu into the unallocated space so that GRUB can handle dual-boot management.
- Disabled Secure Boot and set boot order via BIOS prior to installations.

OS Installs
1. Installed Windows 11 Home first (for easier bootloader control)
   - Activated Windows using a purchased product key during the installation process.
   - Also signed in with my Microsoft account during setup, but no linked licence was associated with this new device.
2. Installed Ubuntu 24.04 LTS into unallocated space using 'Something else' partitioning:
   - Created an 80 GB ext4 partition for `/`
   - Installed GRUB bootloader to the root NVMe drive (`/dev/nvme0n1`)
3. Verified successful dual-boot: GRUB displays both Ubuntu and Windows options on boot.
   - To switch between OSes, I use the BIOS boot menu (F2 on Framework Laptop).

Post-Install Tasks
- Installed drivers for both OS environments
- Verified Linux kernel compatibility with Framework hardware

## 🛠️ Troubleshooting & Fixes

Issue | Fix
--- | ---
BIOS not detecting bootable USB | Recreated ISO with Rufus and used USB 2.0 port
Ventoy install failing mid-process on 32 GB USB (Windows unable to format stick afterwards) | Tried admin rights, DiskPart clean, and antivirus exclusions — still failed. Likely ageing USB controller or bad sectors. Ordered two new 32 GB sticks; plan to retry Ventoy on one and use the second for a dedicated Windows 11 installer via Rufus as a backup.
Older USB not bootable after Ventoy attempt | Wiped partitions with DiskPart; still unstable — confirmed media replacement was the best resolution.
AV deleting Ventoy zip on download | Switched to downloading from official GitHub release page (v1.1.05) to avoid false positive triggers.

## 📚 Lessons Learned from USB Prep

- **Hardware Reliability Matters** – Even if a USB stick works for basic storage, it may fail when subjected to repeated raw writes during bootloader installation.  
- **Always Have Redundancy** – A secondary USB for a dedicated Windows installer ensures you’re never stranded, even if your multi-boot stick fails.  
- **Source Trust** – Downloading from official GitHub releases prevented antivirus false positives and guaranteed file authenticity.  
- **Layered Troubleshooting** – Systematically tried permission elevation, drive wipes, and AV exclusions before concluding the issue was hardware-related.  
- **Time Management Under Constraints** – Adapted plan quickly knowing PC access was time-limited before return, ensuring new media was ordered in time.  

🧠 Skills Demonstrated
- Hardware Assembly – RAM, SSD, chassis reassembly
- BIOS Configuration – Boot order, Secure Boot, UEFI settings
- OS Installation & Partitioning – Bootable USBs, dual-boot via GRUB
- Troubleshooting – Firmware, drivers, bootloader issues
- Documentation – Structured technical record with photos and notes
- Cross-Platform Administration – Managing and supporting both Windows and Linux at hardware level

📷 Photos / Screenshots Gallery (Optional)
- BIOS screen / boot order
- Dual-boot GRUB menu
- Ubuntu desktop
- Windows 11 desktop

🪞 Reflection

Choosing dual-channel 2×16 GB RAM over a single 32 GB stick was intentional — this setup offers better memory bandwidth immediately, improving performance for VMs, multitasking, and OS testing. I can still upgrade later and recoup some cost by selling the current kit.

Upgrading to a 2 TB high-performance NVMe SSD was also deliberate. It balances performance gains with practical capacity needs for a dual-boot environment, giving ~150% more usable space after OS/tool installs compared to a 1 TB drive, and ensuring the system remains future-proof for my lab work and experiments.

The dual-boot configuration is central to the project’s learning value:
- It reflects real enterprise scenarios where both Windows and Linux systems are in use.
- It enables full-speed, bare-metal Linux use without virtualisation overhead.
- It allows me to practice hardware-level admin tasks like partitioning, bootloader setup, and recovery processes.
- It demonstrates my ability to plan, implement, and troubleshoot a multi-OS environment — skills directly relevant to cross-platform IT roles.


CPU Choice Update:
Initially, I considered the Intel Core Ultra 5 125H, but ultimately chose the Ultra 7 155H. 
The rationale was simple: for only an ~8% increase in cost, the Ultra 7 provides stronger multi-threaded performance, 
higher efficiency for demanding workloads, and greater longevity for a dual-boot development environment. 
This ensures I won’t need to upgrade as soon, while keeping total system cost in check.
## 📝 Corrections & Variations

- 🧠 **RAM Slot Orientation:** Contrary to the general expectation, Channel 0 on this unit was on the **left-hand side**, with the **reversed notch**. This required inserting the DDR5 stick with **chips facing down** on the left side — a notable variation that other builders should visually confirm before applying pressure.
- 🧹 **
Next Steps:
- Test battery life, performance benchmarks, and thermals
- Explore scripting for automated updates/backups (PowerShell + Bash)
- Share a refined version of this documentation on LinkedIn and GitHub


🎉 **Project Completed**

This marks the successful completion of my Framework Laptop 13 DIY Build & Dual-Boot Project. I’ve:
- Assembled a modular laptop from components
- Performed a secure and intentional Windows 11 + Ubuntu dual-boot setup
- Demonstrated skills in system imaging, partitioning, BIOS config, OS installation, troubleshooting, and documentation

This project now serves as both a practical training experience and a portfolio artefact for roles involving IT support, infrastructure, and system administration.
