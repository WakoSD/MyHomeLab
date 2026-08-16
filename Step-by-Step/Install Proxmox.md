# Proxmox VE 9.2 – Bootable USB Creation Guide

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

Once the system boots from the USB:
- Select the **Graphic Installer** option for Proxmox VE.
- Continue with the installation process normally.

<img width="1060" height="409" alt="imagen" src="https://github.com/user-attachments/assets/27a7092b-7cba-4ed2-99ba-5d40cc50c59a" />

It will load some information, we just need to wait there. 

---
## ISSUE ENCOUNTERED
Even when we enabled VTx is showing an error message.
<img width="1594" height="670" alt="imagen" src="https://github.com/user-attachments/assets/7bce1c54-0b7a-4cd5-9d4a-5a8222828fd7" />
## Solution:
The option Virtualization Technology (VT‑x) was not enabled correctly for some reason. It did not saved when I did it the first time. Make sure it is enabled. 
---




