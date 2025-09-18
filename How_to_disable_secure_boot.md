# How to disable Secure Boot

### What is Secure Boot?
Secure Boot is a security feature built into modern computers.  
It makes sure that, when your PC starts, only operating systems signed by Microsoft (like Windows) are allowed to boot.  
That way, malware hiding in the boot process can’t easily start.

### Why disable it?
- Most Linux distributions can boot with Secure Boot enabled, but some drivers or installers **don’t work properly** unless Secure Boot is disabled.  
- With Ventoy: if you created the USB **without `-s` option**, Secure Boot must be turned off to boot.  
- Even if you used Ventoy **with `-s`**, sometimes it’s easier to turn Secure Boot off permanently after Linux installation, because hardware drivers usually work more smoothly that way.

**Important:** **Disabling Secure Boot will **not** make your PC unsafe in normal everyday use.  
It just means you can install and boot Linux, or use Ventoy freely. You can always turn it back on later.**

---

### How to enter BIOS/UEFI
When you power on your PC/laptop, immediately press the right key repeatedly until BIOS/UEFI opens.  
Typical keys: **F2, F10, F12, Esc, Del**.  

If one doesn’t work, restart and try another. Many laptops even show a short message like *“Press F2 for Setup”* or *“Press DEL for BIOS”*.

---

### Common manufacturers – BIOS key and Secure Boot menu

| Manufacturer        | Keys to enter BIOS/UEFI                           | Where to disable Secure Boot                                             | Notes                                                                 |
|---------------------|---------------------------------------------------|--------------------------------------------------------------------------|-----------------------------------------------------------------------|
| **Acer**           | F2, Del                                           | `Boot` → Secure Boot                                                     | Sometimes you must first set a Supervisor Password to unlock option.   |
| **ASUS**           | F2, Del                                           | `Boot` → Secure Boot → OS Type = Other OS                                | Changing OS Type disables Secure Boot.                                |
| **Dell**           | F2, F12                                           | `Boot` tab                                                               | Toggle Secure Boot = Disabled.                                        |
| **HP**             | Esc → F10, or F10 directly                        | `System Configuration` → Boot Options                                    | Sometimes you must enable Legacy Support.                             |
| **Lenovo ThinkPad**| F1 (sometimes Enter first)                         | `Security` → Secure Boot                                                 | Option in Security tab.                                               |
| **Lenovo IdeaPad** | F2, Enter                                         | `Security` or `Boot` tab                                                 | Similar to ThinkPad.                                                  |
| **MSI**            | Del                                               | `Settings` → Advanced → Windows OS Configuration → Secure Boot           | Toggle there.                                                         |
| **Samsung**        | F2, F10                                           | `Boot` → Secure Boot                                                     | Standard toggle.                                                      |
| **Toshiba**        | F2, Esc                                           | `Security` → Secure Boot                                                 | Change to Disabled.                                                   |
| **Sony VAIO**      | F2, Assist button                                 | `Boot` → Secure Boot                                                     | Use Assist button if F2 doesn’t work.                                 |
| **Microsoft Surface** | Volume Up + Power                              | `Security` → Secure Boot Control                                         | In UEFI menu.                                                         |
| **Gigabyte (desktops)** | Del                                          | `BIOS Features` → Secure Boot                                            | Set OS Type = Other OS.                                               |
| **Intel NUC**      | F2                                                | `Boot` → Secure Boot                                                     | Simple toggle.                                                        |
| **Fujitsu**        | F2, F12                                           | `Security` → Secure Boot                                                 | Standard option.                                                      |
| **Medion**         | F2, Del                                           | `Boot` → Secure Boot                                                     | Same layout as Insyde BIOS.                                           |
| **Packard Bell**   | F2                                                | `Boot` → Secure Boot                                                     | Similar to Acer (same OEM BIOS).                                      |
| **Huawei (MateBook)** | F2, Esc                                        | `Security` or `Boot` → Secure Boot                                       | Sometimes under Boot Configuration.                                   |

---

### If Secure Boot option is greyed out
- Set a **Supervisor/Administrator password** in BIOS. That unlocks hidden settings.  
- After disabling Secure Boot, you can remove the password again.  
- Sometimes you must set **Boot Mode = UEFI only** or enable **Legacy/CSM** before Secure Boot can be changed.

---

### In short
1. Restart your computer.  
2. Enter BIOS/UEFI with the right key (see table above).  
3. Find **Secure Boot** under *Boot* / *Security* / *Authentication* tab.  
4. Change setting to **Disabled**.  
5. Save and exit (usually F10).  

Now you can boot Linux or Ventoy USB without problems if ventoy was installed with no support for secure boot.
