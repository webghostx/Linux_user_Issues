# Change MAC Address for Dual Boot System

## Overview
In a dual boot setup with Windows and Linux, change the MAC address in Linux to differentiate it from Windows in the router.

### System Configuration
- **Operating Systems:** Windows 11 and Ubuntu 24.04
- **Boot Manager:** GRUB managed via Grub2Win on Windows
- **Network Interface:** enp12s0 (Original MAC: D8:43:AE:49:A6:48)
- **New MAC Address:** 00:00:00:00:A1:C3

### Solution
Create a script in Ubuntu to change the MAC address. This change is temporary but will be applied at every startup.

### Steps

1. **Create the Script:**
   Create a script in Ubuntu to change the MAC address.

   **Script Path:** `/home/yves/.sys/set-mac.sh`

   **Script Content:**
  ```bash
  #!/bin/bash
  set -x
  exec > /dev/console 2>&1
  
  if ip link set dev enp12s0 address 00:00:00:00:a1:c3; then
      echo "set-mac.sh: MAC-Adresse erfolgreich geändert."
  else
      echo "set-mac.sh: Fehler beim Ändern der MAC-Adresse."
  fi
  
  exec /sbin/init
  ```
  Make sure the script is executable:
  `chmod +x /home/yves/.sys/set-mac.sh`

2. **Update GRUB Boot Parameters:**
   Adjust the GRUB boot parameters on the Windows side using Grub2Win.

   **GRUB2 Boot Parameter:**
   `verbose  init=/home/yves/.sys/set-mac.sh -- `

### Important Notes
- The script must exist, otherwise, the boot process will hang.
- Attempts to create a check using `/bin/bash` to verify the script's existence have been unsuccessful so far.

