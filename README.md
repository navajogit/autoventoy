# autoventoy - quick Ventoy installer for USB (Debian-like / Linux)
Automatically install the latest Ventoy USB bootloader on a chosen USB stick.

**Ventoy is an open source multi-image bootloader that gives your USB stick a big advantage over other boot tools**:  
- you’re not locked to a single ISO image. Instead, you can copy as many ISOs as you want — Linux distros, Windows installers in different versions, recovery utilities — all at once (ISO/WIM/IMG/VHD(x)/EFI files).  
- At boot, Ventoy shows you an interactive menu where you just pick what you want to run or install.  
- **The best part:** the USB stick still works like a normal drive. You can keep your personal files and documents alongside the ISOs, so one device doubles as portable storage **and** a multi-boot installer.  
MORE at: https://www.ventoy.net/

**Normally installing Ventoy is a pain in the butt (the process isn’t obvious and the steps are clunky) BUT**: this script is an auto-installer that makes it effortless — in just a few seconds you have Ventoy ready to go. After that, all you do is drag ISO files onto the stick.

---

## REMOTE RUN (no cloning)

If you don’t want to clone or download anything locally, run the script straight from GitHub.  
You’ll still be asked to confirm the target USB before anything destructive happens.

---

### With Secure Boot support (default, recommended)

```
    curl -fsSL "https://raw.githubusercontent.com/navajogit/autoventoy/main/autoventoy" | bash
```

This installs Ventoy **with Secure Boot support** (`-s`). - Works even if Secure Boot is **enabled in BIOS/UEFI**.  
- On first boot you’ll just need to **accept the Ventoy key once**.  
- After that, all ISOs boot normally, no BIOS changes required.  
- If your PC already has Secure Boot disabled, the stick still works fine.

---

### Without Secure Boot support

```
    curl -fsSL "https://raw.githubusercontent.com/navajogit/autoventoy/main/autoventoy" | bash -s -- --no-secure
```

This installs Ventoy **without Secure Boot support**.  
- Simpler technically, but it means you must **manually disable Secure Boot in BIOS/UEFI** to boot from this stick.  
- See here for step-by-step guide: [How to disable Secure Boot](./How_to_disable_secure_boot.md).

---

### More options

```
    curl -fsSL "https://raw.githubusercontent.com/navajogit/autoventoy/main/autoventoy" | bash -s -- -h
```

---

### Notes
- `-fsSL` makes `curl` fail fast and stay quiet on errors.  
- Always read remote scripts before piping to `bash` if you care about security.  
- The script will interactively ask you to choose the USB drive from the list (or you can pass `-d /dev/sdX`).  

---


## LOCAL INSTALL (put the script in your PATH)

Download the script to `/usr/local/bin` and make it executable:

```
sudo curl -fsSL -o /usr/local/bin/autoventoy "https://raw.githubusercontent.com/navajogit/autoventoy/main/autoventoy"

sudo chmod +x /usr/local/bin/autoventoy
```

**Run it:** `autoventoy`

**Get help with all the options:** `autoventoy -h`

---

# What this script does

- Fetches the **latest Ventoy release** directly from GitHub.  
- Falls back to a local file `ventoy-*-linux.tar.gz` (in `.` or `~/Downloads`) if no internet.  
- Validates and extracts the package.  
- Detects all USB drives connected to your system.  
  - If there’s more than one, you’ll get an interactive menu (fzf).  
  - Or you can auto-pick the only USB, or specify it manually with `-d /dev/sdX`.  
- Runs the official `Ventoy2Disk.sh` installer with the correct options.  

**Everything happens in seconds — just follow the prompts.**  

---

## Why this makes life easier

Normally, you’d have to:  
1. Search GitHub for the latest Ventoy release.  
2. Download and unpack the archive.  
3. Find the right installer script.  
4. Manually figure out which disk is your USB stick.  

This script automates all of it, does sanity checks, and helps you avoid wiping the wrong disk.  

---

## Default settings

By default the script runs Ventoy with:

- **Action: install** – the USB stick will be completely formatted and Ventoy will be installed from scratch. All data on the stick will be erased. Use this when preparing a new Ventoy stick.  

- **Secure Boot support enabled (default)** – the USB will work even if Secure Boot is turned on in the BIOS.  
  The first time you boot from it, the system will ask you to accept the Ventoy certificate, and after that it will boot normally.  
  If Secure Boot is already disabled, the stick will also work fine.  

  **Note:** Ventoy itself works fine with Secure Boot, but **after installing a Linux distribution on your hard drive you will usually need to disable Secure Boot in the BIOS**, because many Linux systems do not run reliably with Secure Boot enabled once installed.  

  See here for a step-by-step guide: [How to disable Secure Boot](./How_to_disable_secure_boot.md).  

  Secure Boot does not need to be disabled if you only want to boot live ISOs from the Ventoy USB.  

- **Partition table: MBR (default)** – Ventoy will create an MBR (Master Boot Record) partition table on the USB.  
  - **Pros:** maximum compatibility with old BIOS systems and UEFI machines, works on most PCs.  
  - **Cons:** cannot handle drives larger than 2 TB and is an older standard.  
  - **Alternative: GPT** – use `--gpt` if your USB is larger than 2 TB or if you only plan to use it on modern UEFI hardware. GPT is cleaner and more flexible, but very old BIOS-only PCs might not boot from it.  


---

### Why these defaults?

- **Install (`-i`)** ensures you always get a clean Ventoy setup.  
- **Secure Boot ON (`-s`)** gives you more flexibility: it doesn’t block anything, and it lets you boot even on locked-down machines.  
- **MBR** gives maximum compatibility across both old and new systems.  

For advanced needs you can switch to `-u` (update instead of install), `--no-secure`, or `--gpt`. But for most users, the defaults are the safest and most universal choice.


**You can change this with options:**  
- `--no-secure` disable Secure Boot.  
- `--gpt` use GPT instead of MBR.  
- `--reserve SIZE_MB` reserve extra space.  
- `--label LABEL` custom partition label.  
- `-u` update Ventoy on the USB.  
- `-I` force installation.  
- `-y` skip confirmation.  
- `-a` auto-pick if only one USB.  
- `-d /dev/sdX` specify the device directly.  

Examples:  

'./autoventoy'  
'./autoventoy -y -d /dev/sdb --gpt --reserve 1024'  
'./autoventoy --update -d /dev/sdb'  

---

# Warnings

- **Installing Ventoy formats the USB drive**. Everything on that drive will be erased.  
- Updating with `-u` is usually safe, but **back up your files first** just in case.  
- Ventoy does **not** stop you from using the USB as normal storage. You can put ISO/WIM/IMG files in the root, and keep your other personal files in folders. You only gain extra bootable functionality.  
- Ventoy changes the partition layout in a way that’s not trivial to undo. The official Ventoy documentation explains recovery methods on their site.  

---

# How to uninstall Ventoy from a USB stick

Ventoy doesn’t really have an “uninstaller.” Once installed, it rewrites the partition table of the USB stick.  
So if you just do a quick format, Ventoy will still be there. To completely remove it you need to wipe and recreate the partition table.  

Here are a few methods:

---

### On Linux

- **Using `dd` (low-level wipe)**  
  Replace `/dev/sdX` with your USB device (double-check, this will erase everything).  

  `sudo dd if=/dev/zero of=/dev/sdX bs=1M conv=notrunc,sync status=progress`

  This writes zeros over the beginning of the drive, erasing Ventoy’s partition table. Afterward you can create a new partition with `gparted` or `fdisk`.  

- **Using GParted (GUI tool)**  
  1. Install gparted if you don’t have it: `sudo apt install gparted`  
  2. Run `sudo gparted`  
  3. Select your USB device from the dropdown in the top right.  
  4. From the menu: `Device → Create Partition Table…` and pick `msdos` (MBR).  
  5. Then create a new FAT32 partition using all available space.  
  6. Apply changes.  

ℹ**Note:** Ventoy sometimes puts the filesystem starting right at sector 0 (no partition table).  
That can confuse partition managers — they may show the stick as “read-only” or “corrupted.”  
The fix is simple: just create a new partition table in GParted (`Device → Create Partition Table`).  
This resets the stick to normal.  

---

### On Windows

- **Using Disk Management**  
  1. Press `Win + R`, type `diskmgmt.msc`, press Enter.  
  2. Find your USB drive in the list (be very careful to pick the right one).  
  3. Right-click on each partition of the USB and choose **Delete Volume**.  
  4. After all partitions are gone, right-click the unallocated space and choose **New Simple Volume**.  
  5. Format it as FAT32 or exFAT.  

- **If the stick shows as read-only / you can’t delete partitions**  
  Sometimes after Ventoy Windows marks the stick as write-protected. You can clear this with `diskpart`:  
  1. Run `cmd` as Administrator.  
  2. Type `diskpart` and press Enter.  
  3. `list disk` → find your USB number.  
  4. `select disk X` (replace `X` with your USB disk number).  
  5. `attributes disk clear readonly`  
  6. Now try deleting partitions in Disk Management again.  

- **Using a third-party tool (e.g. Rufus, MiniTool, AOMEI)**  
  Tools like Rufus or MiniTool Partition Wizard can also wipe the USB and create a new partition table. Choose **MBR + FAT32** for maximum compatibility.  

---

### Summary

- **Quick format is not enough.** You need to wipe or recreate the partition table.  
- **Linux fix is easiest:** GParted → *Device → Create Partition Table (msdos)* → new FAT32 partition.  
- **Windows fix may need extra steps:** use Disk Management, and if the drive is read-only use `diskpart` to clear the attribute.  
- In rare cases, a stick can really fail (hardware lock in read-only mode). Then only manufacturer tools or warranty replacement can help.  


---
### Removing the Ventoy Secure Boot key from PC

When you install Ventoy with the default Secure Boot option, the first time you boot from that USB your computer asks you to accept a Ventoy “key” (a small certificate).  
This is normal and harmless — it’s just the way Secure Boot allows Ventoy to run.  

But maybe later you decide you don’t want that key on your computer anymore.  
Deleting it is easy — Ventoy provides a special ISO file just for that.  

**Step by step (simple version):**
1. Download the ISO that removes the Ventoy Secure Boot key:  
   - From GitHub: [DeleteVentoySecureBootKey release v1.0](https://github.com/ventoy/DeleteVentoySecureBootKey/releases/tag/v1.0)  
   - Or from the official docs: [ventoy.net delete key page](https://www.ventoy.net/en/doc_delete_key.html)  
2. Copy that ISO file onto your Ventoy USB stick (just like you copy movies or music).  
3. Restart your computer and boot from the Ventoy USB stick.  
4. In the Ventoy menu that appears, choose the ISO you just copied.  
5. Follow the short instructions on screen.  

That’s it — the Ventoy key will be removed from your computer’s Secure Boot list.  

So in plain words: if you ever said “yes” to the Ventoy key when booting with Secure Boot on, and you want to undo that decision, you just boot Ventoy again with the special ISO and it cleans it up for you.

---

# Dependencies

On Debian/Ubuntu most of these are preinstalled:  

- Required: `lsblk`, `findmnt` (util-linux), `awk`, `grep`, `sed`, `cut`, `sort`, `realpath` (coreutils etc.), `find` (findutils), `tar`.  
- For online fetching: `curl`.  
- Optional but very handy: `fzf` (disk picker), `jq` (parse GitHub API).  
- `sudo` if you’re not root.  

The script can suggest installing missing packages with `apt` on Debian-like systems.  

---

# Bottom line

Run it with no options, pick your USB, type `YES`, and you’re done.  
Ventoy will be installed in seconds and your stick will boot any ISO you drop onto it.  
For beginners, it’s the fastest way to turn a USB stick into a multi-boot toolbox.  
