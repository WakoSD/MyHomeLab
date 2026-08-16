# Proxmox VE 9.2 – Installation Guide 

## 1. Download Proxmox VE
- Opened your browser and go to:  
  **https://www.proxmox.com/en/downloads/proxmox-virtual-environment**
- Download the latest image, in my case:  
  **Proxmox VE 9.2 ISO Installer**

<img width="1406" height="396" alt="imagen" src="https://github.com/user-attachments/assets/35904596-ec3f-4113-a91a-0b6d39dd98d7" />

## 2. Download Rufus
- Visit **https://rufus.ie**
- Download the latest version of **Rufus** that works for your system.
- Open Rufus once downloaded

## 3. Prepare Your USB Drive
- Insert the USB drive you will use for the installation.
- ⚠️ **Important:** This process will erase all data on the USB.

## 4. Create the Bootable USB with Rufus
- In Rufus:
  - **Device:** Select your USB drive.
  - **Boot selection:** Click **SELECT** and choose the `Proxmox VE` ISO you downloaded.
<img width="513" height="613" alt="imagen" src="https://github.com/user-attachments/assets/7f0eee09-a10f-4bb6-9c0c-37fc31833a18" />

- Click **START**.
- If prompted about ISO mode vs DD mode, choose **ISO mode** (recommended).
- Wait for Rufus to finish writing the image.

## 5. Safely Remove the USB
- Once Rufus shows **READY**, close the program.
- Safely eject your USB drive.

Your **Proxmox VE bootable USB** is now ready for installation.

## 6. Prepare the Mini PC (BIOS Configuration)

Before installing Proxmox, you need to configure the mini PC and enable hardware virtualization.

### 6.1 Enter the Startup Menu / BIOS
- Power on the mini PC.
- **Spam the F1 key** (Or the key that allows to enter BIOS) during boot to open the **Startup Menu**.
- From the menu, select **BIOS Setup**.

### 6.2 Enable Virtualization Technology (VT‑x)
Inside the BIOS:
- Go to the **Advanced** tab.
- Open **System Options**.
- Enable **Virtualization Technology (VT‑x)**.

#### Why is VT‑x important?
- Proxmox is a **hypervisor**, meaning it relies on hardware virtualization to run virtual machines efficiently.
- VT‑x provides:
  - CPU‑accelerated virtualization  
  - Support for KVM/QEMU  
  - Better performance and stability for VMs  
  - Lower overhead compared to software‑only virtualization  
- Without VT‑x, Proxmox can boot, but **you cannot run proper virtual machines**, and many features will be limited or unavailable.

## 7. Boot from the Proxmox USB Installer

### 7.1 Open the Boot Menu
- Restart the mini PC.
- **Spam F1 again** to open the Startup Menu.
- This time, select **Boot Menu** (Proxmox did not boot automatically in my case).

### 7.2 Select the USB Drive
- In the Boot Menu, change the boot priority.
- Choose your **Proxmox VE 9.2 bootable USB** as the primary boot device.

## 8. Start the Proxmox Installer

### 8.1 Once the system boots from the USB:
- Select the **Graphic Installer** option for Proxmox VE.
- Continue with the installation process normally.

<img width="1060" height="409" alt="imagen" src="https://github.com/user-attachments/assets/27a7092b-7cba-4ed2-99ba-5d40cc50c59a" />

It will load some information, we just need to wait there. 

### 8.2 Accept the license (Always read it of course)
<img width="992" height="620" alt="imagen" src="https://github.com/user-attachments/assets/93d31024-3711-45a5-b463-5d6725630e2a" />

### 8.3 Select the disk that works for you.
<img width="988" height="512" alt="imagen" src="https://github.com/user-attachments/assets/c8dc02f2-d7f1-4668-9c0b-5d8698dd2b9b" />

### 8.4 Select the country, time zone and keyboard layout
<img width="974" height="470" alt="imagen" src="https://github.com/user-attachments/assets/e630feb0-38a8-4f2e-b5db-e5f282cf9d78" />

### 8.5 Create a password and select an email. It needs to be a valid and working email. 
<img width="1031" height="524" alt="imagen" src="https://github.com/user-attachments/assets/d1323bbf-9044-40d1-8714-bdf318dc14c0" />

### 8.6 We have to assign a hostname. In my case pve0.local and assigned the IPs. You can use the defaults. 
<img width="938" height="928" alt="imagen" src="https://github.com/user-attachments/assets/a428c77a-e895-4d02-8f48-72df874c3bc0" />

### 8.7 You will receive a summary. Please check the info and click install. 
<img width="987" height="608" alt="imagen" src="https://github.com/user-attachments/assets/87297393-88b9-4f27-a77c-a1a3f56eeb35" />

Let it load for a bit until it asks your for your root account and password. Is ready to configure. 

## 9. Access
You need to use root and password created before. 

---
# Troubleshooting
## 1. Even when we enabled VTx is showing an error message.
<img width="1594" height="670" alt="imagen" src="https://github.com/user-attachments/assets/7bce1c54-0b7a-4cd5-9d4a-5a8222828fd7" />
## Solution:
The option Virtualization Technology (VT‑x) was not enabled correctly for some reason. It did not saved when I did it the first time. Make sure it is enabled. 

## 2. Unable to access the Proxmox Web GUI 
<img width="1148" height="561" alt="imagen" src="https://github.com/user-attachments/assets/bfe03363-e933-4543-9243-ab41ddf8137c" />

* **Root Cause 1 (Subnet Mismatch):** The Proxmox server was set to a different subnet than the management PC so unable to get to the address and wrong gateway, preventing direct communication.
* **Root Cause 2 (Host File Desync):** Although the network interface (`/etc/network/interfaces`) was updated to the new IP, the local hostname file (`/etc/hosts`) still pointed to the old IP. This desync caused internal Proxmox proxy services (`pveproxy`) to fail or hang, keeping the GUI inaccessible.

## Solution

1. **Fixed the Network Subnet:**
   Accessed the server's physical console (or active shell) and update `/etc/network/interfaces` to match the correct local network range and gateway:
2. **Update the Hosts File:**
    Edit /etc/hosts and update the node's IP address to match the new configuration.
3. Restarted Services:
    Applied the changes and restarted the required services to bring the web interface back online:
   ```
    systemctl restart networking
    systemctl restart pveproxy
   ```
4. Accessed GUI:
    Navigated to the new address on my browser and it worked.
   <img width="1178" height="655" alt="imagen" src="https://github.com/user-attachments/assets/24b8fb7a-0fab-4930-b744-cae9c2bc0f58" />


