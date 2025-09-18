# autoventoy — quick Ventoy installer for USB (Debian-like / Linux)
Automatically install the latest Ventoy USB bootloader on a chosen USB stick.


All in one: **fast Ventoy installation on a USB stick with interactive disk selection**.  
Just plug in your USB drive and run one command — the script will do the rest.
## Remote install (no cloning)

If you don’t want to clone or download anything locally, run the script straight from GitHub. You’ll still be asked to confirm the target USB before anything destructive happens.

**Use this one-liner:**

`curl -fsSL "https://raw.githubusercontent.com/navajogit/autoventoy/main/autoventoy" | bash`

**Notes:**
- `-fsSL` makes `curl` fail fast and stay quiet on errors.
- Always read remote scripts before piping to `bash` if you care about security.
- The script will interactively ask you to choose the USB drive from the list (or you can pass `-d /dev/sdX`).

---

## Local install (put the script in your PATH)

Download the script to `/usr/local/bin` and make it executable:

```sudo curl -fsSL -o /usr/local/bin/autoventoy "https://raw.githubusercontent.com/navajogit/autoventoy/main/autoventoy"`

sudo chmod +x /usr/local/bin/autoventoy
```

**Run it:** `autoventoy`

**Get help:** `autoventoy -h`

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

- Action: install (`-i`).  
- Secure Boot: **ON** (`-s`).  
- Partition table: **MBR**.  

You can change this with options:  
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
- Ventoy changes the partition layout in a way that’s not trivial to undo. The official Ventoy documentation explains recovery methods on their site.  
- Ventoy does **not** stop you from using the USB as normal storage. You can put ISO/WIM/IMG files in the root, and keep your other personal files in folders. You only gain extra bootable functionality.  

---

## Dependencies

On Debian/Ubuntu most of these are preinstalled:  

- Required: `lsblk`, `findmnt` (util-linux), `awk`, `grep`, `sed`, `cut`, `sort`, `realpath` (coreutils etc.), `find` (findutils), `tar`.  
- For online fetching: `curl`.  
- Optional but very handy: `fzf` (disk picker), `jq` (parse GitHub API).  
- `sudo` if you’re not root.  

The script can suggest installing missing packages with `apt` on Debian-like systems.  

---

## Bottom line

Run it with no options, pick your USB, type `YES`, and you’re done.  
Ventoy will be installed in seconds and your stick will boot any ISO you drop onto it.  
For beginners, it’s the fastest way to turn a USB stick into a multi-boot toolbox.  
