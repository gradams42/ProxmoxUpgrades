# **Proxmox VE: Enhanced Tag-Based VM Organization**  
**Version:** 1.0  
**Author:** Gabriel Adams  
**Date:** March 2025  

---

## **Overview**  
This update enhances the **Proxmox Virtual Environment (Proxmox VE)** UI by introducing an **automated, structured folder system** based on **VM and container tags**.  
Instead of a flat list of resources, this feature dynamically organizes VMs into a **nested directory structure** based on their assigned tags.  

This helps administrators efficiently manage **large-scale Proxmox environments** by allowing for **hierarchical categorization of resources**.

---
![Demonstration](Organized_Node.png)

## **Key Features**  

### **Tag-Based Folder Organization**  
- The **first tag** assigned to a VM/LXC **determines its parent folder**.  
- Additional tags create **subdirectories** under that parent.  
- Has remediations for accidental incorrect ordering (e.g., ensures `Production -> Database` instead of `Database -> Production`).
   
- Users should currently use an **A), B), C)** naming scheme when creating subdirectory tags to prevent improper sorting (e.g., `A-Production`, `B-Database`).  
For example, when I make 'Production' and 'Test' directories that are separate, I will make A-Production and A-Test tags. Then, lets say I want a database and a WWW tag under both. The second tags I add to the VM's in those folders will be b-database, and b-www. This will ensure that VM's inside of say Production --> database are in the subfolder database which is under production. 

### **Real-Time Updates (No Full Refresh Needed)**  
- The tree dynamically **refreshes when tags change**, eliminating the need for a **full page reload**.  
- Immediate feedback on **folder structure updates**.  

### **Automatic Cleanup**  
- When a VM **loses all tags**, it **moves back to the default** folder.  
- Empty folders **are automatically removed**.  

### **Duplicate Prevention**  
- Ensures VMs **do not appear in multiple locations**, unlike in the current tagging functionality.  

---

## **Advantages**  

| Feature            | Before Update          | After Update                      |
|--------------------|----------------------|----------------------------------|
| **VM Organization** | Flat list of VMs     | Structured folders based on tags |
| **Folder Naming**  | Some folders unnamed | Folders inherit tag names        |
| **Tag Validation** | Tags assigned inconsistently | Logical folder hierarchy enforced |
| **Tree Updates**   | Required full page refresh | Updates in real time |
| **Empty Folders**  | Persisted indefinitely | Removed automatically |
| **Duplicates**     | VMs could appear multiple times | VMs appear only in one location |

---

## 🔧 **How to Apply the Update**  

### **Step 1: Backup Your Existing File**  
Before making any modifications, **backup your current `pvemanagerlib.js`**:  

```sh
cp /usr/share/pve-manager/js/pvemanagerlib.js /usr/share/pve-manager/js/pvemanagerlib.js.bak
```

This ensures that you can **restore the original file** if needed.  

---

### **Step 2: Replace `pvemanagerlib.js`**  
Clone this repository with:
```sh
git clone https://github.com/gradams42/ProxmoxUpgrades.git'
cd ProxmoxUpgrades
```

1. **Upload the new file** to your Proxmox VE server:  
   ```sh
   scp pvemanagerlib.js root@your-proxmox-ip:/usr/share/pve-manager/js/
   ```

   ```
2. **Ensure correct permissions**:  
   ```sh
   chmod 644 /usr/share/pve-manager/js/pvemanagerlib.js
   ```

---

### **Step 3: Restart the Proxmox Web Interface**  
For the changes to take effect, restart the **Proxmox web interface and daemon**:  

```sh
systemctl restart pveproxy
systemctl restart pvedaemon
```

### **Step 4: Refresh the Page**  
Use **`CTRL + Shift + R`** to reload any cached frontend files in your browser.

---

## **Future Improvements**  

- **Fixing real-time updates**:  
  - Currently, the user must **refresh** after adding or removing tags to see updates.  
  - Future updates will make this process **seamless**.  

- **Improving tag sorting logic**:  
  - Proxmox **automatically sorts tags alphabetically**, which **interferes with the intended hierarchical structure**.  
  - I am working on updating **how tag data is sent to the backend** to **preserve order** based on **creation time instead of alphabetical sorting**.  

- **Better integration with existing Proxmox workflows**:  
  - Ensure this update is compatible with **Proxmox's native API**.  
  - Minimize UI conflicts for **users who prefer standard tag-based organization**.  

---

## **Credits**  
- **Developed by:** Gabriel Adams  
- **Who this is for:** Open Source Proxmox Community
